title: 对linux文件权限的理解
date: 2016-08-03 08:06:02
categories: [linux,权限]
tags: [权限,命令]
---

*nix系统中，文件的各种权限比较重要，以下内容，是通过学习《鸟哥的linxu私房菜》之后总结的内容，方便自己理解和记忆。这篇文章最早发布于[我的博客园](http://www.cnblogs.com/ilvu/p/4002992.html)。

文件和目录都有权限：r、w 、x，并且对于 拥有者（属主），同组用户，其他用户 都有对应的权限限制。
* 对于文件：r代表读，可读取文件的内容；w代表写，可修改文件的内容；x代表执行，可以执行文件
* 对于目录（记录文件清单）：r代表可以列出其中文件的相关信息；w代表目录下文件和目录的删除、重命名、新建；x代表可以以该目录作为工作目录
 
总结：
* 文件的访问及使用限制由文件本身的权限决定，但是文件名称的修改、文件本身的删除是由文件所在目录的w权限决定的
* 文件的拥有着或者root可以修改文件或者目录的权限

<!-- more --> 
 
显然的，对于所有以文件的方式存在的命令，它们也有权限，所以，比如cd、ls等系统提供的命令也是有权限的，可以通过删除其x权限禁止用户使用该命令。
用户使用系统：即用户通过一个程序去操作文件或档案，要顺利进行这一动作，得满足两个条件：

* 对该程序文件的执行权限，如对 cd 、ls 等的x权限
* 对要操作的目标文件的访问权限，如r，w

举例：用户 dvid 进行操作：cat readme.md，首先 dvid 得具有 cat 程式的执行权限：x，其次 dvid 得具有 readme.md 的读权限：r。具体见下图
{% qnimg 2016-08-06-linux-authority.png title:linux文件权限演示 alt:linux文件权限演示 'class:' %}
