---
layout: post
title: "cocoa中使用chmod()修改文件权限"
date: 2014-04-23 16:44:00 
comments: true
categories: [cocoa]
tags: [cocoa]
description: 在cocoa中，有时需要通过代码来修改文件权限，因为Mac OSX支持标准的POSIX接口，所以可以使用chmod()函数来修改文件权限。
keywords: cocoa chmod

---

在cocoa中，有时需要通过代码来修改文件权限，因为Mac OSX支持标准的POSIX接口，所以可以使用chmod()函数来修改文件权限。

{% highlight c %}
int	chmod(const char *, mode_t)

/* File mode */
/* Read, write, execute/search by owner */
#define	S_IRWXU		0000700		/* [XSI] RWX mask for owner */
#define	S_IRUSR		0000400		/* [XSI] R for owner */
#define	S_IWUSR		0000200		/* [XSI] W for owner */
#define	S_IXUSR		0000100		/* [XSI] X for owner */
/* Read, write, execute/search by group */
#define	S_IRWXG		0000070		/* [XSI] RWX mask for group */
#define	S_IRGRP		0000040		/* [XSI] R for group */
#define	S_IWGRP		0000020		/* [XSI] W for group */
#define	S_IXGRP		0000010		/* [XSI] X for group */
/* Read, write, execute/search by others */
#define	S_IRWXO		0000007		/* [XSI] RWX mask for other */
#define	S_IROTH		0000004		/* [XSI] R for other */
#define	S_IWOTH		0000002		/* [XSI] W for other */
#define	S_IXOTH		0000001		/* [XSI] X for other */

#define	S_ISUID		0004000		/* [XSI] set user id on execution */
#define	S_ISGID		0002000		/* [XSI] set group id on execution */
#define	S_ISVTX		0001000		/* [XSI] directory restrcted delete */
{% endhighlight %}

例如：修改一个文件的权限

	chmod([[self filePath] UTF8String], S_IRUSR|S_IWUSR|S_IRGRP|S_IWGRP|S_IROTH|S_IWOTH);
 


