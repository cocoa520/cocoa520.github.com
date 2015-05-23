---
layout: post
title: Adb工具介绍
date: 2014-09-08 23:19:00 
comments: true
categories: [开源工具]
tags: [Android]
description: 做Android开发的，必定要接触Adb,它是一个跨平台的功能强大的调试工具，让人想起了Gdb。
keywords: Adb Android SDK Debug Bridge

---

由于需要在Mac上操作Android设备，所以要了解一下Adb工具，Adb是Android SDK中的一部分，如果到Google Android官网下载的话，没法单独下载，可以直接到百度搜一个下载，在使用之后，发现Adb真是无所不能，下面记录一下。

#### Adb是什么

Adb的全称为Android Debug Bridge，中文是Android设备调试桥的作用,也就是Android开发的Debug工具，不过，要想使用Adb，必须在手机的`开发者选项` 中打开`USB调试`模式。

Adb是Android sdk里的一个工具, 用这个工具可以直接操作管理android模拟器或者真实的android设备。

它的主要功能有:

* 运行设备的shell(命令行)
* 管理模拟器或设备的端口映射
* 计算机和设备之间上传/下载文件
* 将本地apk软件安装至模拟器或android设备

Adb是一个客户端-服务器端程序, 其中客户端就是用来操作的电脑, 服务器端是Android设备,搞Android的，注定离不开Adb.

官方网址：[Adb详解](http://developer.android.com/tools/help/adb.html)<br/>
下载地址：[Adb源码下载](http://open.karfield.com/adb/)

#### 常用Adb命令

	1. 查看设备 

		adb devices
这个命令是查看当前连接的设备, 连接到计算机的android设备或者模拟器将会列出显示。

	2. 指定设备
	
		adb -s <serialNumber> <command> 
通过指定设备或模拟器的序列号，执行adb命令。

	3. 安装软件

		adb install <apk文件路径>
这个命令将指定的apk文件安装到设备上

	4. 卸载软件

		adb uninstall <软件名>
		adb uninstall -k <软件名>
如果加 -k 参数,为卸载软件但是保留配置和缓存文件.

	5. 登录设备shell

		adb shell
		adb shell <command命令>
这个命令将登录设备的shell.后面加<command命令>将是直接运行设备命令, 相当于执行远程命令。

	6. 从电脑上发送文件到设备

		adb push <本地路径> <远程路径>
用push命令可以把本机电脑上的文件或者文件夹复制到设备(手机)。

	7. 从设备上下载文件到电脑
		
		adb pull <远程路径> <本地路径>
用pull命令可以把设备(手机)上的文件或者文件夹复制到本机电脑。

	8. 显示帮助信息

		adb help
		
在使用Adb的命令时，确保手机的USB调试模式已经打开，Adb是跨平台的程序，命令很多，非常的强大。


#### Adb扩展

由于Android设备种类繁多，而Adb支持的设备有限，如果你想扩展对一些设备的连接，可以修改Adb源码，然后重新编译Adb。
这里有一个网站提供了一些帮助：[http://open.karfield.com/adb/](http://open.karfield.com/adb/)
