---
layout: post
title: "如何判断CFArray中对象的类类型"
date: 2014-08-11 22:24:00 
comments: true
categories: [cocoa]
tags: [cocoa]
description: 在Foundation框架中，我们很容易判断一个对象是什么类型，那么在CoreFoundation框架中，如何判断CFArray中对象的类类型。
keywords: cocoa

---

在Foundation框架中，我们很容易判断一个对象是什么类型的，因为NSObject中封装了判断的方法，如下所示：

{% highlight objective-c %} 

- (BOOL)isKindOfClass:(Class)aClass;

{% endhighlight %}

但是在CoreFoundation框架中，就没有提供这样的方法。

当我们调用方法获取CFArray中的对象时，如下
 
{% highlight objective-c %}

CF_EXPORT const void *CFArrayGetValueAtIndex(CFArrayRef theArray, CFIndex idx);

{% endhighlight %}
 
返回的类型是void*，如果这个数组不是我们自己添加的数据，我们就有可能不知道它里面装的对象是什么类型，如果统一用某一类型去强转，肯定会出问题，那有什么办法判断对象类型呢。
 
通过查看CF的API，发现了常用的数据类型中都存在这样一个函数：

{% highlight objective-c %}

CF_EXPORT CFTypeID CFArrayGetTypeID(void);
CF_EXPORT CFTypeID CFStringGetTypeID(void);
CF_EXPORT CFTypeID CFDictionaryGetTypeID(void);

{% endhighlight %}
 
这个CFTypeID是一个无符号长整形值，估计是标识这个数据类型的，后面在CFBase.h头文件中，又找到了这样一个函数：
 
{% highlight objective-c %}

CF_EXPORT CFTypeID CFGetTypeID(CFTypeRef cf);
 
{% endhighlight %} 

这样一来，就很明显了，CF框架就是通过这样的方式来判断数据类型。

{% highlight objective-c %}
 
CFArrayRef theArray = NULL;
if(CFGetTypeID(CFArrayGetValueAtIndex(theArray, 0)) == CFDictionaryGetTypeID())
{
  NSLog(@"is CFDictionaryRef");
}
{% endhighlight %} 