title: AnglarJS Tips（持续更新）
date: 2016-05-28 17:16:19
categories: [JavaScript,AngularJS]
tags: [angularjs,javascript,双向绑定,脏数据检测]
---

用AngularJS的人越来越多了，这里有一些重要的问题，可以作为对AngularJS的考察点。
以下内容，针对1.x版本。

1.AngularJS实现双向绑定的原理是什么？
2.$scope上$apply的作用是什么，什么时候需要你手动调用？
3.使用AngularJS的好处是什么？

以上问题，实际涉及到AngularJS的核心原理，能否清楚的回答，直接反映出你对AngularJS了解程度的深浅。

<!-- more --> 

AngularJS作为一个MVC框架，一个很重要的功能就是数据绑定，即模型（model，具体体现为$scope对象）和视图（view，具体体现为指令模板）之间的数据同步，双向绑定呢，既有从模型到视图，也有视图到模型。我们使用ng-model这个指令来说明双向绑定的核心原理。
比如模板 &lt;input ng-model="model" /&gt;， 数据从视图到模型这个方向，ng-model指令通过在input这个元素上添加事件监听（change），当input的 值发生变化的时候，新值被写入$scope.model属性；对于数据从模型到视图这个方向，ng-model指令在$scope上进行了数据监听($watch)，当监测到$scope.model被修改（比如ajax从后台获取数据），那么这个值就会被写入input的value。这就是双向绑定的整个过程。

经过上面的讨论，对于问题1，它的答案是：***数据监听，视图到模型方向通过给模板中的DOM元素添加事件监听（典型的是change）完成，模型到视图方向通过给模型$scope添加属性监听（$watch）实现，如此就可以实现数据在视图和模型之间的双向绑定了。***

上面说到，模型到视图是通过在$scope上添加属性监听（$watch）实现的，那这里有个问题，这个事件监听是如何来实现监测对的呢，它是如何感知到$scope上属性的变化的呢，具体如何实现？视图到模型好说，最终可以通过DOM事件实现，那模型上呢？模型说到底就是一个JavaScript对象，它如何监测自己的变化？
在AngularJS的实现中，采用了一种称之为***“脏数据检测”($digest，也有翻译“脏值检查”)***的机制，即通过$scope的$watch方法，添加好多的检测表达式，以及变化时候的回调，然后在触发“脏数据检测”的时候，逐一执行这些检测表达式，比较这些表达式的值和上一次执行时候的值是否相同来确定$scope是否发生了变化，如果变化了，执行相应的回调方法，这样就能够对$scope的变化做出反应。那好，这里重点是它的实现方式，很显然，$scope还需要被触发去执行这个“脏数据检测”的过程，那么这个检测过程是什么时候被执行的呢？这就是AngularJS的核心原理了。
现在来说下“脏数据检测”的触发过程。“脏数据检测”是发生在每一个具体的$scope对象上的，对于某个$scope而言，进行“脏数据检测”实际上就是执行该$scope上的$digest方法。$scope上还有一个方法就是$apply,$apply是接受一个表达式，并在$scope上执行，最后会调用$digest，即触发“脏数据检测”。大多数时候，AngularJS会自行触发这个“脏数据检测”的过程，并不需要我们手动去调用$digest或者$apply，这也就是为什么我们在使用ng-model或者$http的时候，视图和模型已经自动关联的原因。
以下场景AngularJS会自动的调用$apply:
* angular启动的时候
* angular事件绑定，如ng-click，ng-dblclick，ng-mousedown，ng-mouseup，ng-mouseover，ng-keydown等等
* input，textarea, select等表单元素（实际上input，textarea，select是angular内部定义的指令，angluar中定义的指令可以有多种使用方式，这种就是“E”）
* angular的部分内置服务，如 $http，$timeout等

经过上面的讨论，我们来回答问题2。***$apply是$scope上的一个方法，作用是在$scope上执行一个表达式，然后调用$digest，触发“脏数据检测”过程，实现模型和视图间的同步以及更新。除了上面所述的几种AngularJS会自动触发“脏数据检测”的场景，如果你在混用jQuery绑定DOM事件，调用外部的回调方法等等，这时候就需要手动调用$apply了。***

对于问题3，主要归纳下，就是：（1）AngularJS是一个MVC模式的框架，你可以分离控制逻辑、数据和渲染过程，各个部分职责明确，更加灵活；（2）AngularJS提供了指令这种强大的方式，让你可以扩展HTML，封装功能强大的独立组件；(3)AngularJS有模块的概念，可以将不同的部分放在不同的模块里面，结构清晰；（4）AngularJS提供了依赖注入的功能，让你可以方便地加载需要的服务或者模块等，并且这让测试变得很容易。

上述涉及到AngularJS核心的东西，大家可以查看源码进行对照（针对1.x版本）。这里附上一个stackoverflow上的链接：[how does the binding and digesting work in AngularJS?](http://stackoverflow.com/questions/12463902/how-does-the-binding-and-digesting-work-in-angularjs)，以示对照。