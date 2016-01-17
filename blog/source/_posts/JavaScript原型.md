title: JavaScript原型
date: 2015-12-15 22:39:17
categories: [JavaScript,原型]
tags: [javascript,prototype,原型]
---

JavaScript中对象的继承可以通过原型来实现，下面主要来说一下原型相关的问题。
以下是一些结论，详见文章内的验证：

*	函数都有prototype属性，由函数构造的对象来继承。
*	普通对象都隐含一个其所继承的原型的属性\_\_proto\_\_，除了Object.prototype这个特殊对象等。
*	所有的函数都是Function类的实例。
*   所有的对象都是Object类的实例。
*   Function类是Object类的实例，Object函数也是Function类的实例。
*	所有的函数都是对象，所以函数既有prototype属性，也有\_\_proto\_\_属性。
*   原型链的最顶端是Object.prototype，该对象向上再没有原型了， Object.prototype.\_\_proto\_\_ === null。
    由于Object.prototype这个对象比较特殊，它没有原型，会导致 Object.prototype instanceof Object 返回 false，这和instanceof的工作方式有关。

<!-- more --> 
我是很早就想对JS的原型链做一个总结，也推荐别人的一篇文章[JavaScript 原型概念深入理解](http://www.codeceo.com/article/javascript-prototype-learn.html)。

以下是验证图：
{% qnimg 2015-12-27-function-prototype.png title:属性在原型链中查找 alt:属性在原型链中查找 'class:' %}
{% qnimg 2015-12-27-function-prototype-2.png title:属性在原型链中查找 alt:属性在原型链中查找 'class:' %}
{% qnimg function-instance.png title:函数都是Function类的实例 alt:函数都是Function类的实例 'class:' %}
{% qnimg object-instance.png title:对象都是Object类的实例 alt:对象都是Object类的实例 'class:' %}
{% qnimg 2015-12-27-function-prototype-4.png title:Function和Object alt:Function和Object 'class:' %}