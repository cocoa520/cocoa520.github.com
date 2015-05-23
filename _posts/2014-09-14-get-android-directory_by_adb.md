---
layout: post
title: 如何通过adb获得Android手机SD卡的路径
date: 2014-09-14 20:19:00 
comments: true
categories: [开源工具]
tags: [Android]
description: 有没有什么方法可以直接获得Android手机SD卡的路径，而不用写个App到手机上，答案是有的，但网上一找，基本上都是面向Android开发的。
keywords: Android SD卡路径

---

前段时间写一个Mac程序，需要获得Android手机SD卡路径，然后传输与管理文件，上网找了找有没有什么方法可以直接获得Android手机SD卡的路径，结果基本上都是面向Android开发的，对于一个Mac程序来说，再写个App放在上面，过于复杂。

后面想到Android设备都是通过adb来操作的，所以找了一下，如何通过adb获得Android手机SD卡的路径，还真有，而且，Android手机SD卡还分内外置两种情况，下面记录一下ADB的操作命令。

关于Adb是什么，以及常用命令的使用可以查看[`Adb工具介绍`](www.cocoa520.com/use_adb_cmdline.md)这篇文章.

* 读取设备的内置SD卡路径，使用Adb Shell的环境变量`$EXTERNAL_STORAGE`

		adb -s "HT********039" shell echo \$EXTERNAL_STORAGE
		/storage/sdcard0
		
		adb -s "Cool****0x02a27402" shell echo \$EXTERNAL_STORAGE
		/storage/emulated/legacy

* 如果设备还有外置SD卡，使用Adb Shell的环境变量`$SECONDARY_STORAGE`

		adb -s "HT********039" shell echo \$SECONDARY_STORAGE
		/storage/sdcard1
		
如果手机里还有第二张外置SD卡，Adb Shell的环境变量会不会是$TERTIARY，到目前我还没有见过这样的手机，如果有谁知道的话，请给我留言。

