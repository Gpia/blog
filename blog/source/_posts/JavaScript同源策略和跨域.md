title: JavaScript同源策略和跨域
date: 2016-08-16 23:52:01
categories: [JavaScript,同源策略]
tags: [javascript,同源策略,跨域]
---

同源策略是对JavaScript代码能够操作哪些Web内容的一条完整的安全限制。当Web页面使用多个iframe元素或者打开其他浏览器窗口的时候，这一策略通常就会发生作用。具体来说，脚本只能读取和所属文档来源相同的窗口和文档的属性。

文档的来源包括：协议，主机，端口。三者有任何一个不同，即为不同来源。需要说明的一点是，文档中的脚本本身的来源和同源策略并不相关，这一点非常重要。例如，一个来自主机A的脚本被包含到（使用script标签的src属性）宿主B的一个页面中，那么，这个脚本的来源是主机B，简单的说，就是脚本本身来自哪里不重要，重要的是脚本被包含在哪个文档里。

实际上，同源策略并非应用于不同源的窗口的所有对象的所有属性，不过他应用到了其中的大部分属性，尤其是Document对象的几乎所有属性。此外，同源策略还会阻止XMLHttpRequest对不同文档来源的请求。

不同来源文档之间的相互访问称为跨域（访问），同源策略能够阻止跨域访问带来的安全问题。

对于使用多个子域的大站点来说，同源策略就显得有点过于严格了，比如home.example.com的文档里的脚本要合法地访问developer.example.com载入的文档的属性。这种情况下，可以通过文档中的脚本设置document的domain属性，使得两个子域的文档的domain是同一个值，这样，这两个窗口就可以不受同源策略的限制了。
当然，domain的值不能是任意值，它只能是当前文档来源域的前缀或者他本身，另外domain中必须有一个点号，不能设置为“com”这样的顶级域名，比如home.example.com可以设置domain为example.com。对于home.example.com和developer.example.com，只要将它们的文档domain设置为example.com就可以进行跨域操作了。

对于XMLHttpRequest的跨域请求，可以通过跨域资源共享（Cross-Origin Resource Sharing，即CORS）来实现。具体是在跨域http请求头部增加Origin属性，对应的响应头部增加Access-Control-Allow-Origin。
当然，对于跨域请求，可以借助JSONP技术，具体即借助script标签的src属性可以加载不同来源的脚本来实现。
另外，通过跨文档信息（cross-document messaging）技术，可以在不同文档的脚本之间传递消息，而不用管脚本的来源是否不同。调动window对象上的postMessage()方法，可以一步传递消息时间（可以用onmessage时间处理程序来处理它）到窗口的文档里。

