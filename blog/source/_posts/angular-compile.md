title: angular的DOM编译过程
categories: [javascript,AngularJS]
tags: [AngularJS,javascript,compile,编译]
---
angular的DOM编译过程总的来说，是编译DOM，生成一个复合链接函数，然后传入scope作为参数，执行该链接函数，在scope和实际的DOM之间建立联系，编译过程结束。
下面主要来说一下编译DOM并生成链接函数的过程，以下分析基于[angular-1.3.0源码](https://github.com/Gpia/temp-data/blob/master/javascript/lib/angular-1.3.0.js)，由于代码很长，不在此贴出，可点击此链接查看，涉及到的内容主要从6094行开始。
编译DOM并生成链接函数的过程是由一个叫做compileNodes的内部方法完成的，该方法从一个指定的待编译节点列表开始编译，对于从document元素上启动的angular应用，那么待编译节点列表从一个只包含document元素（内部使用的是document的包装类型，即jqLite(document)）的数组开始，形如 [jqLite(document)]。

一.具体来说compileNodes的过程：

1.对于待编译节点列表nodeList中的每一个节点node，收集它上面的指令（collectDirectives），然后对该节点node应用指令（applyDirectivesToNode），调用applyDirectivesToNode方法会返回一个当前node的链接函数nodeLinkFn；接着，如果有子节点，则重复步骤1，并传入子节点列表childNodes作为参数，生成childNodeLinkFn。
2.将nodeLinkFn和childLinkFn加入一个linkFns数组，保存（linkFns.push(i, nodeLinkFn, childLinkFn)，i是node在nodeList中的索引）即linkFns中包含了待编译节点列表中每一个node的nodeLinkFn和childLinkFn。
3.返回一个复合链接函数compositeLinkFn。

通过以上3步compileNodes已经执行结束，此过程中DOM中指令的编译过程已经完成，即已经完成了指令的变换（applyDirectivesToNode方法），比如用指令的模板替换指令的占位元素。

还通过上述步骤，compileNodes返回的是一个复合链接函数compositeLinkFn，applyDirectivesToNode返回的是一个普通的链接函数nodeLinkFn，
复合链接函数用于执行当前节点node的nodeLinkFn和当前节点子节点的childLinkFn，其中由于childLinkFn是由compileNodes的递归调用返回的，所以它是一个复合链接函数，而当前节点的nodeLinkFn是由applyDirectivesToNode返回的，是一个普通的链接函数。即compositeLinkFn的执行到最后都是nodeLinkFn的执行。

<!-- more --> 

实际上，上述步骤3返回的链接函数构成了一个闭包，它在执行时能够访问到上面的linkFns。

二.上面步骤3返回的的复合链接函数的执行过程（传入参数：scope,nodeList，rootElement。）

1.遍历linkFns，每一次遍历，都取出一个节点索引i，nodeLinkFn（普通链接函数）和childLinkFn（复合链接函数），通过nodeList[i]获取nodeLinkFn对应的节点node。
2.对于当前节点node，
  如果有nodeLinkFn，则首先根据nodeLinkFn包含的信息nodeLinkFn.scope生成恰当的子scope，如果有子scope，则childScope=scope.$new()，否则childScope=scope，然后执行nodeLinkFn，即nodeLinkFn(childLinkFn, childScope, node, $rootElement, childBoundTranscludeFn);
  如果没有nodeLinkFn，但有childLinkFn，则执行childLinkFn，即回到步骤1，继续执行子复合链接函数： childLinkFn(scope, node.childNodes, undefined, parentBoundTranscludeFn)。
3.执行结束。

重点说明：compileNodes方法处理的是模板DOM（对应compileNode），nodeLinkFn方法执行时是针对已经是编译后的DOM（对应linkNode），那么问题来了，linkFns数组中保存的node索引i，是在模板DOM中的顺序，而compositeLinkFn执行时却根据它从编译后的DOM中取node，对应不上？首先，对于有模板的指令，模板只能有一个根节点，即compileNode和linkNode是一一对应的，对于没有模板的指令，则compileNode===linkNode。再关键在于applyDirectivesToNode方法，该方法处理compileNode并最终返回了一个能够处理linkNode的nodeLinkFn，所以在nodeLinkFn在 最后执行的时候，既能够访问到compileNode（闭包），也能够访问到其该compileNode在变换之后对应的linkNode（参数），它在内部做了处理。

由上面我们知道，复合链接函数compileLinkFn的执行最后都是nodeLinkFn的执行，而nodeLinkFn是由applyDirectivesToNode方法返回的，顾名思义，它将指令应用到节点，完成了模板DOM到最终DOM的转换。

三.applyDirectivesToNode的执行过程(待完成)

四.nodeLinkFn的执行过程(待完成)