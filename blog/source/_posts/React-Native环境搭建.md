title: React Native环境搭建
date: 2016-07-14 23:28:33
categories: [JavaScript,React]
tags: [javascript,react,react native]
---

最近尝试了下React Native，[官网的安装教程](https://facebook.github.io/react-native/docs/getting-started.html#content)，已经写的非常详细了，但是我在环境配置的过程中，还是发现了一些大坑。

***重点是watchman的版本得是4.0以上！***

如果你按找官方的教程，搭建好了Android或者IOS的开发环境，安装了[watchman](https://facebook.github.io/watchman)，在使用react native跑示例程序的时候，报错，出现一些奇怪的报错，那么很大可能就是这个原因！尤其是mac，在可能已经安装了watchman的情况下，需要卸载老版本，重新安装最新的版本。

<!-- more --> 

在mac上，可以使用Homebrew安装最新版watchman的命令是：
```Bash
watchman -v  #查看watchman的版本
brew uninstall watchman  #卸载
brew unlink watchman  
brew install --HEAD watchman  #通过github直接安装最新版本的watchman
```

如果是开发React Native的Android程序，那么需要先搭建Android开发环境，如果是开发IOS的程序，需要先搭建IOS的开发环境。
按照教程上的，如果是开发Android应用，那么需要配置Android开发环境，当然最方便的就是，直接下载Android官方开发工具[Android Studio](https://developer.android.com/studio/install.html)，如果不能翻墙上不去，就去这里找一找[http://www.androiddevtools.cn/](http://www.androiddevtools.cn/)。然后打开里面的Android SDK Manager下载相应的sdk、模拟器镜像等等，并将Android SDK的路径加入用户环境变量，用于在命令行被调用。注意MAC和windows平台上环境变量的设置。

对于Android方式，借助Android Studio可以方便地创建安卓模拟器、启动模拟器，然后，在其上跑React Native的应用。
对于IOS，好像只能是在mac上开发，有了xcode，安装ios模拟器之后，就和安卓的方式一样了。

下面列出一些Android开发常用的命令（来自android sdk，请确保android sdk路径已经被加入path环境变量），比如通过命令行创建模拟器或者启动模拟器，虚拟机即模拟器。
```Bash
android list avd  #查看所有的虚拟机
android create avd  #创建虚拟机
emulator -avd avd-name  #启动虚拟机，avd-name 不要带后缀.avd
adb devices  #查看已经启动的安卓设备（包括模拟器和通过usb连接的手机）
```

