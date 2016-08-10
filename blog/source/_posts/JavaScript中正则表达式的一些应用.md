title: JavaScript中正则表达式的一些应用
date: 2016-08-10 22:51:34
categories: [正则表达式,应用]
tags: [正则表达式,JavaScript]
---

前面讲了正则表达式的基础知识[正则表达式入门](/2016/08/07/正则表达式入门/)，以及[JavaScript对正则表达的支持](/2016/08/07/JavaScript对正则表达式的支持/)之后，现在来看下正则表达式的应用实例。

对数字进行千分位分隔
```js
function numberFormat(num) {
    return String(num).replace(/\d(?=(\d{3})+$)/g, '$&,');
}
console.log(numberFormat(1234567));  // 1,234,567
```
获取url参数
```js
function getUrlParam(name) {
    var str = location.search,
        reg = new RegExp('[?&]' + name + '=' + '([^&]*)'),
        result;
    result = reg.exec(str);
    return result ? decodeURIComponent(result[1]) : undefined;
}
```
一个简单的字符串模板方法，模板使用{{}}进行占位，可替换传入的对象的属性，可多层次引用。
```js
function formatTpl(tpl, data) {

```
