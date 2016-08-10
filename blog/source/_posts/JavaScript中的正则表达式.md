title: JavaScript中的正则表达式
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




