---
layout: post
title: "cocoa获取系统序列号，uuid及Mac地址"
date: 2014-04-24 11:33:00 
comments: true
categories: [cocoa]
tags: [cocoa]
description: 在Mac OS X系统中，可以在关于本机中查看硬件系统，也可以通过cocoa代码来获取系统序列号，uuid及Mac地址。
keywords: cocoa 序列号 UUID Mac地址

---


  在Mac os x系统中，我们可以通过点击屏幕上方的苹果菜单，选中关于本机，然后再点更多信息，系统报告查看硬件系统，比如我的Mac 信息：
 
#### 硬件概览

	型号名称：iMac
	型号标识符：iMac9,1
	处理器名称：Intel Core 2 Duo
	处理器速度：2.66 GHz
	处理器数目：1
	核总数：2
	二级缓存：6 MB
	内存：4 GB
	总线速度：1.07 GHz
	Boot ROM版本：IM91.008D.B08
	SMC版本（系统）：1.44f0
	序列号（系统）：17XXXXXXXTF
	硬件UUID：D0XXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXFB
 
 最后两项序列号和UUID，用处比较大，以下大概讲一下它们在系统的用涂吧。
 	
	序列号：系统唯一标识标，和window系统的用途差不多，可以用来验证一些注册相关的问题。
 	
	硬件UUID：系统用得比较多，典型的就是ByHost特定硬件偏好设置，比如，屏幕保护的偏好配置，我的是:com.apple.screensaver.D0XXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXFB.plist，Mac OS X系统就是靠这个偏好配置文件来启动对应程序的。当然我们也可以用它验证很多东西。
  
  注：深入了解的话，可以查看[http://blog.csdn.net/afatgoat/article/details/4302337](http://blog.csdn.net/afatgoat/article/details/4302337)
 
#### 代码读取
 接下来讲解一下，如果通过代码来获取，可以使用两种方法。
 
* 使用ioreg命令
* 使用IOKit框架
 
 
下面详细分析如何实现。
 
###### 一、使用ioreg命令管道

在Terminal中执行下面的命令 ioreg -d2 -c IOPlatformExpertDevice，我们可以得到以下信息：
  
{% highlight objective-c %}

<class IOPlatformExpertDevice, id 0x100000110, registered, matched, active, b
{
	"compatible" = <"iMac9,1">;
	"version" = <"1.0">;
	"board-id" = <"Mac-F2218EA9">;
	"IOInterruptSpecifiers" = (<0900000005000000>;)
	"IOPolledInterface" = "SMCPolledInterface is not serializable"
	"serial-number" = <30544600000000000000000000313739343330303430544600000000000000000>
	"IOInterruptControllers" = ("io-apic-0")
	"IOPlatformUUID" = "D0XXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXFB"
	"IOPlatformArgs" = <0060880280ffffff0010880280ffffff18de8c0280ffffff0000000000000000>
	"clock-frequency" = <005a6b3f>;
	"manufacturer" = <"Apple Inc.">;
	"IOConsoleSecurityInterest" = "IOCommand is not serializable"
	"IOPlatformSerialNumber" = "17XXXXXXXTF"
	"system-type" = <01>;
	"product-name" = <"iMac9,1">;
	"model" = <"iMac9,1">;
	"name" = <"/">;
	"IOBusyInterest" = "IOCommand is not serializable"
}

{% endhighlight %}  
 
IOPlatformUUID与IOPlatformSerialNumber就是我们想要找的值，下面使用cocoa的管道来获取，代码如下：
  
{% highlight objective-c %}

- (NSString *)GetHardwareUUID
{
	NSTask *task;
	task = [[NSTask alloc] init];
	[task setLaunchPath: @"/usr/sbin/ioreg"];
	//ioreg -rd1 -c IOPlatformExpertDevice | grep -E '(UUID)'
	
	NSArray *arguments;
	arguments = [NSArray arrayWithObjects: @"-rd1", @"-c",@"IOPlatformExpertDevice",nil];
	[task setArguments: arguments];
	NSPipe *pipe;
	pipe = [NSPipe pipe];
	[task setStandardOutput: pipe];
	NSFileHandle *file;
	file = [pipe fileHandleForReading];
	[task launch];
	NSData *data;
	data = [file readDataToEndOfFile];
	NSString *string;
	string = [[NSString alloc] initWithData: data encoding: NSUTF8StringEncoding];
	//NSLog (@"grep returned:n%@", string);
	 
	NSString *key = [NSString stringWithString:@"IOPlatformUUID"];
	NSRange range = [string rangeOfString:key];
	NSInteger location = range.location + [key length] + 5;
	NSInteger length = 32 + 4;
	range.location = location;
	range.length = length;
	NSString *UUID = [string substringWithRange:range];
	UUID = [UUID stringByReplacingOccurrencesOfString:@"-" withString:@""];
	//NSLog(@"UIID:%@",UUID);
	return UUID;
}

{% endhighlight %}
 
IOPlatformSerialNumber的获取与以一样，只要修改对应的字符串即可。
  
###### 二、使用IOKit框架来获取
   
获取时，需要导入系统的IOKit.framework.代码如下：

{% highlight objective-c %}  
 
NSString* GetHardwareUUID()
{
	NSString *ret = nil;
	io_service_t platformExpert ;
	platformExpert = IOServiceGetMatchingService(kIOMasterPortDefault, IOServiceMatching("IOPlatformExpertDevice")) ;
	if (platformExpert)
		{
		CFTypeRef serialNumberAsCFString ;
		serialNumberAsCFString = IORegistryEntryCreateCFProperty(platformExpert, CFSTR("IOPlatformUUID"), kCFAllocatorDefault, 0) ;
		if (serialNumberAsCFString) {
			ret = [(NSString *)(CFStringRef)serialNumberAsCFString copy];
			CFRelease(serialNumberAsCFString); serialNumberAsCFString = NULL;
		}
		IOObjectRelease(platformExpert); platformExpert = 0;
	}
	return [ret autorelease];
}  

NSString * GetHardwareSerialNumber()
{
	NSString * ret = nil;
	io_service_t platformExpert ;
	platformExpert = IOServiceGetMatchingService(kIOMasterPortDefault, IOServiceMatching("IOPlatformExpertDevice")) ;
	if (platformExpert)	{
		CFTypeRef uuidNumberAsCFString ;
		uuidNumberAsCFString = IORegistryEntryCreateCFProperty(platformExpert, CFSTR("IOPlatformSerialNumber"), kCFAllocatorDefault, 0) ;
		if (uuidNumberAsCFString)	{
			ret = [(NSString *)(CFStringRef)uuidNumberAsCFString copy];
			CFRelease(uuidNumberAsCFString); uuidNumberAsCFString = NULL;
		}
		IOObjectRelease(platformExpert); platformExpert = 0;
	}
	return [ret autorelease];
}

{% endhighlight %} 
  
接下来，对于Mac地址的获取，可以参考前面的链接，里面有介绍。

最后介绍一下iOS型号，版本，内存，磁盘，MAC地址等信息获取，可以参考uidevice-extension开源库，地址是：
   
    https://github.com/erica/uidevice-extension
   
  
####  参考链接：
 
 * [http://www.jaharmi.com/2008/03/15/get_uuid_for_mac_with_ioreg](http://www.jaharmi.com/2008/03/15/get_uuid_for_mac_with_ioreg)
 * [http://blog.sina.com.cn/s/blog_50e0bce501018bmf.html](http://blog.sina.com.cn/s/blog_50e0bce501018bmf.html)
   
    
   
  
  
 


