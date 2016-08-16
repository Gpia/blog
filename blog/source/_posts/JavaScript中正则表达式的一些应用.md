title: JavaScript中正则表达式的一些应用
date: 2016-08-10 22:51:34
categories: [正则表达式,应用]
tags: [正则表达式,javascript]
---

前面讲了正则表达式的基础知识[正则表达式入门](/2016/08/07/正则表达式入门/)，以及[JavaScript对正则表达的支持](/2016/08/07/JavaScript对正则表达式的支持/)之后，现在来看下正则表达式的应用实例。

1. 对数字进行千分位分隔
```js
//说明：对于数字中的任何一位，如果其后面有3，6，9，... 位数字，那么该位后面就应该有一个,
function numberFormat(num) {
    return String(num).replace(/\d(?=(\d{3})+$)/g, '$&,');
}
console.log(numberFormat(1234567));  // 1,234,567
```
2. 获取url参数
<!-- more --> 
```js
//说明：url查询字符串以 ? 开头，参数以 & 分隔，参数组如 key=value
function getUrlParam(name) {
    var str = location.search,
        reg = new RegExp('[?&]' + name + '=' + '([^&]*)'),
        result;
    result = reg.exec(str);
    return result ? decodeURIComponent(result[1]) : undefined;
}
```
3. 获取cookie值
```js
//说明：cookie 以 '; ' 分隔
function getCookie(name) {
    var reg = new RegExp('(?:^| )' + name + '=([^;]*)(;|$)'),
        result = reg.exec(document.cookie);
    return result ? decodeURIComponent(result[1]) : undefined;
}
```
4. 日期格式化
```js
//说明：接收一个 date 类型，一个字符串 format，如：yyyy-MM-dd HH:mm:ss
function dateFormat(date, format) {
    var o = {
        'y+': date.getFullYear(),
        'M+': date.getMonth() + 1,
        'd+': date.getDate(),
        'H+': date.getHours(),
        'h+': date.getHours() % 12,
        'm+': date.getMinutes(),
        's+': date.getSeconds()
    }, reg, tmp; 
    for (var f in o) {
        reg = new RegExp(f, 'g');
        tmp = addPreZero(o[f]);
        format = format.replace(reg, function (m) {
            return tmp.slice(tmp.length - m.length);
        });
    }	
    return format;
    function addPreZero(n) {
        return String(n < 10 ? ('0' + n) : n);
    }
}
```
5. IP4地址校验
```js
//说明：ipv4 四个部分以.分隔，每部分从0到255
function isIpAddr(ip) {
    return (((\d{1,2})|(1\d{2})|(2[0-4]\d)|(25[0-5]))\.){3}((\d{1,2})|(1\d{2})|(2[0-4]\d)|(25[0-5])).test(ip);
}
```
6. 11位手机号码校验
```js
//说明：现在手机号以 13,15,17,18 开头的11位数字
function validPhoneNumber(number) {
    return /^1[3578]\d{9}$/.test(String(number));
}
```




