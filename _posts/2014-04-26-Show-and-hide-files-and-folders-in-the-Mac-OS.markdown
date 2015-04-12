---
layout: post
title: "Mac OS中显示及隐藏文件和文件夹的方法"
date: 2014-04-26 15:55:00 
comments: true
categories: [实用技巧]
tags: [实用技巧]
description: Mac有一个设计原则，就是用户不需要看到的或者用户不希望看到的，都会隐藏在背后，如果我们需要显示Mac OSX中的隐藏文件，可以用以下的方法显示及隐藏文件和文件夹。
keywords: 隐藏文件 AppleShowAllFiles

---

  Mac有一个设计原则，就是用户不需要看到的或者用户不希望看到的，都不会显示出来。但如果你想要修改其中某些文件，也是可以的，先需要显示所有的文件，可以在终端中输入命令行来实现。
 
 
#### 显示系统隐藏的文件
 
	defaults write com.apple.finder AppleShowAllFiles -bool true
	KillAll Finder
或者

	defaults write com.apple.finder AppleShowAllFiles YES
	KillAll Finder
 
 
同样可以把这些显示的隐藏文件再隐藏起来
	
	defaults write com.apple.finder AppleShowAllFiles -bool false
	KillAll Finder

或者
	
	defaults write com.apple.finder AppleShowAllFiles NO
	KillAll Finder
 
#### 修改文件隐藏属性

在显示了所有文件之后，那如何把一个不是隐藏的文件修改为隐藏呢，下面介绍两种简单的方法。

  方法一：直接在文件或文件夹名前面的加一个‘.’点号，然后系统会弹出修改确认对话框，点好就行了。
  
  方法二：利用命令“chflags hidden” 可以隐藏文件或文件夹。
 
  先打开Terminal（Applications/Utilities/Terminal），然后执行命令`chflags hidden` 文件路径 或 chflags hidden 文件夹路径既可。也可以先输入`chflags hidden`，然后直接把要隐藏的文件用鼠标选中拖到输入框中，它自动会转换为文件的路径。
  
  要解除文件的隐藏状态，可以使用命令：`chflags nohidden`与前面相对应即可。
 
上面两种方式，到底有啥区别呢，个人觉得方法一并没有修改文件本身的属性，在linux及unix中，约定好点开头的文件就是隐藏文件。
  
方法二中修改的是文件本身的隐藏标志，但貌似在windows上不起作用，只能在linux及unix中有用。而方法一，windows也是可以显示隐藏的，应该是windows也识别linux及unix中的点号约定。
 
在网上还发现有一个可以隐藏文件及文件夹的开源工程，提供了一个带界面的程序，可以方便不会使用命令行的用户，地址是[https://code.google.com/p/hideme4mac/](https://code.google.com/p/hideme4mac/)

最后，附加上chflags命令的详细信息。
  
  chflags 命令修改文件的标志（change file flags），包括隐藏标志，其详细使用方法如下：
  
{% highlight bash %}

SYNOPSIS
chflags [-fhv] [-R [-H | -L | -P]] flags file ...

DESCRIPTION
The chflags utility modifies the file flags of the listed files as specified by the flags operand.

The options are as follows:

-f Do not display a diagnostic message if chflags could not modify the flags for file, nor modify
the exit status to reflect such failures.

-H If the -R option is specified, symbolic links on the command line are followed. (Symbolic
links encountered in the tree traversal are not followed.)

-h If the file is a symbolic link, change the file flags of the link itself rather than the file
to which it points.

-L If the -R option is specified, all symbolic links are followed.

-P If the -R option is specified, no symbolic links are followed. This is the default.

-R Change the file flags for the file hierarchies rooted in the files instead of just the files
themselves.

-v Cause chflags to be verbose, showing filenames as the flags are modified. If the -v option is
specified more than once, the old and new flags of the file will also be printed, in octal
notation.

The flags are specified as an octal number or a comma separated list of keywords. The following key-
words are currently defined:

arch, archived
set the archived flag (super-user only)

opaque set the opaque flag (owner or super-user only). [Directory is opaque when viewed through
a union mount]

nodump set the nodump flag (owner or super-user only)

sappnd, sappend
set the system append-only flag (super-user only)

schg, schange, simmutable
set the system immutable flag (super-user only)

uappnd, uappend
set the user append-only flag (owner or super-user only)

uchg, uchange, uimmutable
set the user immutable flag (owner or super-user only)

hidden set the hidden flag [Hide item from GUI]

As discussed in chflags(2), the sappnd and schg flags may only be unset when the system is in single-
user mode.

Putting the letters "no" before or removing the letters "no" from a keyword causes the flag to be
cleared. For example:

nouchg clear the user immutable flag (owner or super-user only)
dump clear the nodump flag (owner or super-user only)

Unless the -H or -L options are given, chflags on a symbolic link always succeeds and has no effect.
The -H, -L and -P options are ignored unless the -R option is specified. In addition, these options
override each other and the command's actions are determined by the last one specified.

You can use "ls -lO" to see the flags of existing files.

{% endhighlight %} 
 
下面提供一个中文版
 
chflags
名称：
chflags – 改变文件的标志
概述：
chflags [-fhv] [-R [-H | -L | -P]] 标志 文件
描述：
工具chflags修改指定文件的文件标志。

选项如下：
-f 如果chflags不能修改文件标志，nor modify the exit status to reflect such failures,则不显示诊断信息。
-H 如果开启-R选项，将改变软连接指向的文件的文件标志（遍历树中的软连接除外)。
-h 如果文件是软连接，只改变该链接的文件标志，而不改变该链接所指向的文件的标志。
-L 如果-R选项开启，将改变所有软连接所指向的文件的文件标志。
-P 如果-R选项开启，将不改变所有软连接所指向的文件的文件标志。这是默认选项。
-R Change the file flags for the file hierachies rooted int the files instead of just the files themselves.
-v 当修改标志时显示文件名。如果 –v 出现两次以上，则以八进制同时显示旧标志和新 标志。

文件标志以一个八进制数或一系列以逗号分隔的关键词来显示。下面是当前定义的关 键词：
arch,archived 
存档文件标志（超级用户独有）
opaque 不透明文件标志（适用于文件所有者或超级用户）
nodump nodump文件标志（适用于文件所有者和超级用户）
sappnd,sappend
仅允许附加 文件标志（超级用户独有）
schg,schange,simmutable
不可更改 文件标志（超级用户独有）
sunlnk,sunlink
不可删除 文件标志（超级用户独有）
uappnd,uappend
只允许用户附加 文件标志（适用于所有者和超级用户）
uchg,uchange,uimmutable
不允许用户更改 文件标志（适用于所有者和超级用户）
uunlnk,uunlink
不允许用户删除 文件标志（适用于所有者和超级用户）

在关键词前面添加或者去除“no”将清除相应的文件标志。例如：
nouchg 清除 不可更改 文件标志（适用于所有者或超级用户）
dump 清除 nodump 文件标志（适用于所有者或超级用户）

八进制数值对应的文件标志：
0 清除所有文件标志
1 nodump 
2 uchg
3 uchg，nodump
4 uappnd
10 opaque
20 uunlnk

Other combinations of keywords may be placed by using the octets assigned.但是，以上这些是最常用的。

你可以使用 “ls -lo”来查看文件的文件标志。

注意：能否改变某些标志依赖于当前内核的安全级别设定。查看security(7)来获得更多的 信息。
退出状态：
成功 0，失败>;0.
实例：
无
参考：
ls(1), chflags(2), stat(2), fts(3), security(7), symlink(7)
标准：
无
历史：
chflags最早出现在4.4BSD当中。
BUGS：
Only a limited number of utilities are chflags aware. Some of these
tools include ls(1), cp(1), find(1), install(1), dump(8), and restore(8).
In particular a tool which is not currently chflags aware is the pax(1)
utility.chio
 
 
 






