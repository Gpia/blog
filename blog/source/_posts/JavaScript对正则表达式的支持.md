title: JavaScript对正则表达式的支持
date: 2016-08-07 18:57:36
categories: [正则表达式,应用]
tags: [正则表达式,JavaScript]
---

正则表达式是一种强大的工具，具体语法请看我的另一篇博客[正则表达式入门](/2016/08/07/正则表达式入门/)，现在我们来看看正则表达式在JavaScript中的应用。
JavaScript对正则表达式的支持主要是由正则表达式类RegExp和字符串类String来提供的。

|  方法  |  所属类  |
| ------ | ------- |
|  exec  |  RegExp |  
|  test  |  RegExp  | 
|  match |  String |  
|  replace | String | 
|  search | String | 
|  split |  String | 
<!-- more --> 
在JavaScript中创建正则表达式对象，有两种方式：
```js
//方法1，使用正则表达式类RegExp
//参数1为模式，参数2为属性（可省略，默认不开启）
//i 忽略大小写，g 全局匹配，m 开启多行匹配
var reg = new RegExp('[ab]c', 'gim');

//方法2，使用正则表达式字面量
//使用/作为模式的开始和结束符号，后面紧接属性，意义同上
var reg = /[ab]c/gim; 
```
有了正则表达式对象，我们可以直接使用正则表达式对象的方法 exec。
exec 方法接收一个字符串作为参数，对这个字符串进行正则匹配，如果没有匹配则返回null，否则返回一个结果数组。返回的数组第一项为整个模式的匹配值，后面依次为各个分组（子表达式）匹配的结果。如果没有开启全局匹配 g，它每次执行，总是返回第一个符合模式的匹配，否则它将每次返回下一个匹配，直到没有符合的匹配返回null，之后重新从第1个匹配开始返回。所以可以通过循环找到一个字符串中所有的匹配。
顺便说一句，这个结果数组，它上面还有两个属性，index：匹配的开始位置，input：原始文本，感兴趣的自行查看，我们先忽略这个。

```js
var str = 'so wo know the number is 100, and total is 666.';
var reg = /(\w*)\s*is\s*(\d+)/; 
var result = reg.exec(str);  //result: ["number is 100", "number", "100"]
result = reg.exec(str);  //再次执行，result依然为 ["number is 100", "number", "100"]

//开启全局匹配
reg = /(\w*)\s*is\s*(\d+)/g;
result = reg.exec(str);  //result: ["number is 100", "number", "100"]
result = reg.exec(str);  //接着执行，result: ["total is 666", "total", "666"]
result = reg.exec(str);  //接着执行，result: null
result = reg.exec(str);  //接着执行，重新开始匹配，result: ["number is 100", "number", "100"]
```
下面，我们通过循环输出所有的匹配
```js
var str = 'so wo know the number is 100, and total is 666.';
var reg = /(\w*)\s*is\s*(\d+)/g; 
var result, allResult = [];
while (result = reg.exec(str)) {
    allResult.push(result);			
}
console.log(allResult);  // allResult: [["number is 100", "number", "100"], ["total is 666", "total", "666"]]
```
至于正则表达式对象的 test 方法，用于测试一个字符串是否匹配这个模式，如果匹配则返回true，否则为false，此时是否全局匹配无影响。
```js
var str = 'so wo know the number is 100, and total is 666.';
var reg = /(\w*)\s*is\s*(\d+)/; 
var reg1 = /not-found/;
console.log(reg.test(str));  //true
console.log(reg1.test(str)); //false
```

接下来看下字符串中支持正则表达式的相关方法，以下这些方法都可以接受一个普通的字符串作为参数，这里不谈。

字符串的 match 方法可以接收一个正则表达式作为参数，在字符串内检索与正则相匹配的内容。当正则表达式没有g标志时，返回结果和 RegExp 的 exec 返回结果相同，即如果找到，则返回一个数组，数组第0项为该模式的匹配文本，后面的项依次是子表达式匹配的内容，该返回数组还有 input 和 index属性（先忽略这个）；当正则表达式有g标志时，返回结果则不同，此时，返回的也是一个数组，但是里面的每一项是该模式的所有匹配文本，不含子表达式信息。如果不匹配，返回null。
```js
var str = 'so wo know the number is 100, and total is 666.';
var reg = /(\w*)\s*is\s*(\d+)/; 
console.log(str.match(reg));  //["number is 100", "number", "100"]

reg = /(\w*)\s*is\s*(\d+)/g;  //加入全局标志
console.log(str.match(reg));  //["number is 100", "total is 666"]
```
字符串的 replace 方法也可以接收一个正则表达式作为第一个参数，第二个参数可以是字符串或者一个方法，当第二个参数是字符串的时候，可以使用一些特定符号：$n，n < 99，表示第n个子表达式对应的匹配，$&表示与正则表达式相匹配的文本，$`是位于匹配子串左侧的文本，$'是位于匹配子串右侧的文本，$$是插入$符号。
当第二个参数是方法的时候，其参数就是 match 时候结果数组依次传入，返回值作为本次匹配的替换结果。如果正则表达式有g标志，则会多次替换，直到全部替换完毕。replace方法会返回替换后的结果，但是对原字符串没有任何影响。
```js
var reg = /(\w+)\s(\w+)/;
var str = 'John Smith';
var newstr = str.replace(reg, '$2, $1');  //使用字符串参数替换
console.log(newstr);  // Smith, John

//下面写一个转换方法，用于将驼峰式命名转换成-连接
function nameFormat(oldName) {
    return oldName.replace(/[A-Z]/g, function (match) {
        return '-' + match.toLowerCase();
    });	
}
console.log(nameFormat('tooLongNameFor'));   //too-long-name-for
```
字符串的 search 方法可以接收一个正则表达式对象作为参数，如果可以匹配，那么返回匹配的起始位置，否则返回-1。
```js
var str = 'so wo know the number is 100, and total is 666.';
var reg = /(\w*)\s*is\s*(\d+)/; 
console.log(str.search(reg));  //15
```
字符串的 split 方法可以接收一个正则表达式对象作为参数，将字符串使用匹配的子串分隔。
```js
var str = 'JavaScript,RegExp match，use';
var reg = /,|\s+|，/;  //将使用英文逗号，中文逗号或者空格分隔的字符串分隔关键词
console.log(str.split(reg));  //["JavaScript", "RegExp", "match", "use"]
```
除了以上内容，还有 RegExp 这个类的一点知识点，每次执行依次正则表达式匹配，那么就会有一些信息被保存在 RegExp 类的静态属性上。RegExp 的 $1 到 $9 保存9个子表达式的匹配文本，lastMatch（简写 $&）是最后一个匹配的串，lastParen（简写 $+）是最后一个子表达式匹配的串，leftContext（简写 $`）是匹配串的左侧的所有文本，rightContext（简写 $'）是匹配串的右侧的所有文本，input 保存当前作用的字符串。

好了，JavaScript中的正则支持大概讲完了。




