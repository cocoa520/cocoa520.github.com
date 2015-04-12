---
layout: post
title: "Cocoa如何Test一个可执行文件"
date: 2014-08-10 22:22:00 
comments: true
categories: [cocoa]
tags: [cocoa]
description: 在Window上，我们很容易判断一个文件是不是可执行文件，一般情况简单的看是否为.exe后缀就行了，但是在Mac系统上,就没有这么简单了，下面介绍一个Cocoa中判断文件是否是可执行文件的方法
keywords: cocoa Mach-O executable

---

在Window上，我们很容易判断一个文件是不是可执行文件，一般情况简单的看是否为.exe后缀就行了，但是在Mac系统上，可执行的文件通常是没有后缀的，所以无法从后缀来简单判断，必须另用方法，下面介绍几种常用的方法。
 
#### 方法一：使用file命令
 
	file Mini
	输出--Mini: Mach-O 64-bit executable x86_64
 
通常Mac的可执行文件为Mach-o格式，64-bit代表是64位可执行程序，x86_64指的是可执行程序可以支持32位与64位CPU架构，executable顾名思义，就是可执行文件。如果要用在程序中，可以使用NSTask来启动这个命令。
 
 
#### 方法二：使用mdls命令
 
 mdls是全称是lists the metadata attributes for the specified file，专门用来查看文件属性的，功能很强大，系统Finder也是用得它。
 
	mdls Mini
	
结果如下：

{% highlight objective-c %}

kMDItemContentCreationDate     = 2013-05-17 14:20:38 +0000
kMDItemContentModificationDate = 2013-05-17 14:20:38 +0000
kMDItemContentType             = "public.unix-executable"
kMDItemContentTypeTree         = (
    "public.unix-executable",
    "public.data",
    "public.item",
    "public.executable"
)
kMDItemDateAdded               = 2013-05-17 14:20:38 +0000
kMDItemDisplayName             = "Mini"
kMDItemFSContentChangeDate     = 2013-05-17 14:20:38 +0000
kMDItemFSCreationDate          = 2013-05-17 14:20:38 +0000
kMDItemFSCreatorCode           = ""
kMDItemFSFinderFlags           = 0
kMDItemFSHasCustomIcon         = 0
kMDItemFSInvisible             = 0
kMDItemFSIsExtensionHidden     = 0
kMDItemFSIsStationery          = 0
kMDItemFSLabel                 = 0
kMDItemFSName                  = "Mini"
kMDItemFSNodeCount             = 13460
kMDItemFSOwnerGroupID          = 20
kMDItemFSOwnerUserID           = 501
kMDItemFSSize                  = 13460
kMDItemFSTypeCode              = ""
kMDItemKind                    = "Unix 可执行文件"
kMDItemLogicalSize             = 13460
kMDItemPhysicalSize            = 16384

{% endhighlight %}

从kMDItemContentType = "public.unix-executable"中可以判断它的类型。如果要在程序中使用，同样也要用NSTask来调用。
 
 
#### 方法三：使用NSWorkSpace的typeOfFile: error:方法来判断
 
{% highlight objective-c %}

NSString* filePath = @"/Users/shyee/Mini";
NSLog(@"%@",[[NSWorkspace sharedWorkspace] typeOfFile:filePath error:NULL]);

{% endhighlight %} 

运行上面语句，将得到值
  
	public.unix-executable，
  
这个值和前面mdls的kMDItemContentType的值是一致，可以判断系统底层用得这值来存文件的类型的。NSFileManager是苹果管理文件的类，它提供的方法里面竟然没有判断文件具体类型的。
 
{% highlight objective-c %}
 
NSLog(@"%@",[[NSFileManager defaultManager] attributesOfItemAtPath:filePath error:NULL]);
 
NSFileCreationDate = "2013-05-17 14:20:38 +0000";
NSFileExtensionHidden = 0;
NSFileGroupOwnerAccountID = 20;
NSFileGroupOwnerAccountName = staff;
NSFileHFSCreatorCode = 0;
NSFileHFSTypeCode = 0;
NSFileModificationDate = "2013-05-17 14:20:38 +0000";
NSFileOwnerAccountID = 501;
NSFileOwnerAccountName = liping;
NSFilePosixPermissions = 493;
NSFileReferenceCount = 1;
NSFileSize = 13460;
NSFileSystemFileNumber = 2446064;
NSFileSystemNumber = 234881029;
NSFileType = NSFileTypeRegular;

{% endhighlight %} 

最后一个NSFileType = NSFileTypeRegular并不能明显判断文件类型。查看NSFileType的类型，如下：

{% highlight objective-c %}
 
NSFileType Attribute Values
These strings are the possible values for the NSFileType attribute key contained in the NSDictionary object returned by attributesOfItemAtPath:error:.
             
NSString * const NSFileTypeDirectory;
NSString * const NSFileTypeRegular;
NSString * const NSFileTypeSymbolicLink;
NSString * const NSFileTypeSocket;
NSString * const NSFileTypeCharacterSpecial;
NSString * const NSFileTypeBlockSpecial;
NSString * const NSFileTypeUnknown;
Constants
NSFileTypeDirectory
Directory
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeRegular
Regular file
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeSymbolicLink
Symbolic link
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeSocket
Socket
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeCharacterSpecial
Character special file
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeBlockSpecial
Block special file
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.
NSFileTypeUnknown
Unknown
Available in Mac OS X v10.0 and later.
Declared in NSFileManager.h.

{% endhighlight %} 

NSFileTypeRegular普通文件类型，说明NSFileManager并没有细化文件类型，功能比较偏上层。

方法其实还有很多，可执行文件通常都有它的结构，有兴趣的话，可以去深入了解Mach-o的格式,这里算是抛砖引玉。