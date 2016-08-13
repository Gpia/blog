title: JavaScript函数节流
date: 2016-08-13 23:50:48
categories: [JavaScript,杂项]
tags: [javascript,函数节流]
---

JavaScript中，经常有一种场景，比如我们要在窗口大小变化的时候，同步调整一下图形的大小，或者在文档滚动的时候判断是否加载数据，简单地说，我们可以直接绑定事件，如下：
```js
function f() {
    //do something
}
$(window).resize(function () {
    f();
});
```
但是，上述代码有可能有问题，由于在每次窗口变化的时候，可能会多次触发resize事件，导致f被执行多次，尤其是当处理的方法比较复杂，执行效率较低，或者我们想在每次窗口变化的时候，只执行一次f，这时候就需要进行“函数节流”了，我们让多次resize“合并”成一次resize，即使用一种方式，让每次窗口大小变化都只触发一次resize（实际上是很多次，可打印log验证），上代码：
```js
//说明：这是一个节流方法，接收第一个参数，是要实际执行的函数，delay是一个延迟时间，即每多少时间作为一个节流周期
function throttle(fn, delay) {
    var timer = null;
    return function () {
        var context = this, args = arguments;
        clearTimeout(timer);
        timer = setTimeout(function () {
            fn.apply(context, args);
        }, delay);
    };
 };
```
有了上面的节流函数，我们的resize事件可以优化成：
```js
$(window).resize(throttle(f, 50));
```
