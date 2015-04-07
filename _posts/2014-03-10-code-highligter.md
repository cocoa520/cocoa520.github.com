---
layout: post
title: jekyll博客代码高亮配置
description: 使用 Pygments 来实现代码高亮，Jekyll 原生支持语法高亮工具 Pygments ，Pygments 支持多种语言高亮, 并且非常方便使用。

---

今天我尝试为个人博客`cocoa520.com`添加代码高亮样式，在网上了解一下，发现大部分都是使用Pygments来配置，因为Jekyll原生支持。当然也有使用js的博客，主要是一部分搞前端开发的朋友。由于对前端开发不熟，当然希望配置越简单越好，直接选择Pygments。

Pygments支持多种语言高亮，也有许多各式各样的样式可供选择，便用也很方便，但是在实际配置过程中，还是遇到了一些小问题，在本地预览时，代码高亮效果始终显示不了，设置各种参数都无济于事，最后无奈传到github中，却发现显示正常了，令人无语，猜想可能与github的解析器有关吧。

下面，我记录一下如何安装Pygments、如何配置以及如何生成代码高亮所需要的文件等，以备以后查看。

### 安装Pygments

在`OS X`中，系统自带了`python`，可以使用`easy_install`工具来安装：
	
	sudo easy_install pygments
	
或者通过 pip 来安装

	sudo easy_install pip
	pip install pygments

其他的系统估计就麻烦一些了，可以到百度上去谷歌上去搜索方法。

### 配置

在Jekyll博客的目录中，打开_config.yml文件，设置`highlighter:  pygments`

> 注意：新版本 Jekyll 中，pygments: true 替换为 highlighter: pygments。

进到我们的网站目录，运行下面代码生成Pygments样式

	$ pygmentize -S default -f html > your/path/pygments.css

生成的样式文件加到我们的网页中

	<link rel="stylesheet" href="/your/path/pygments.css">

当然，我们也可以直接生成所需的html文件，如果你用类似Jekyll、hexo等静态网站生成工具，你就不需要自己生成html文件。
假设需要高亮的代码为一段js代码，文件名为test.js：

	var testStr = "hello world";
你需要在终端中输入：

	pygmentize -f html -o test.html test.js
* `-f html`指明需要输出html文件
* `-o test.html`指明输出的文件名
* `test.js`就是输入文件了

最后我们得到的html文件大概是这个样子的：
				
	<div class="highlight"><pre><span class="kd">var</span><span class="nx">testStr</span> <span 	class="o">=</span> <span class="s2">&quot;hello world&quot;</span><span class="p">;</span></pre></div>

### 使用

语法高亮的代码片段要放在标签对 {% highlight language linenos %} 和 {% endhighlight %} 之间，其中的 language 为多种语言高亮页面中的 Short names , linenos为是否显示代码行数的标记。

	{% highlight c %}
	/* hello world demo */
	#include <stdio.h>
	int main(int argc, char **argv)
	{
    	printf("Hello, World!\n");
    	return 0;
	}
	{% endhighlight %}

加入代码行标记：

	{% highlight c linenos %}
	/* hello world demo */
	#include <stdio.h>
	int main(int argc, char **argv)
	{
    	printf("Hello, World!\n");
    	return 0;
	}
	{% endhighlight %}

还有一种markdown GFM fenced code 代码块写法：

	```c
	/* hello world demo */
	#include <stdio.h>
	int main(int argc, char **argv)
	{
    	printf("Hello, World!\n");
    	return 0;
	}
	```
这种写法需要在_config.yml文件中加上参数：

	kramdown:
	input:      GFM

###Pygments 样式
Pygments 样式 默认提供了 monokai、manni、rrt、perldoc、borland、colorful、default 等等，多种的样式。

可以通过以下命令列出当前 Pygments 支持的样式：

	>>> from pygments.styles import STYLE_MAP
	>>> STYLE_MAP.keys()
	['monokai', 'manni', 'rrt', 'perldoc', 'borland', 	'colorful', 'default', 'murphy', 'vs', 'trac', 'tango', 	'fruity', 'autumn', 'bw', 'emacs', 'vim', 'pastie', 	'friendly', 'native']
通过 -S 来选择，譬如要生成 monokai 的样式：

	pygmentize -S monokai -f html > your/path/pygments.css
下面是一些 pygments 样式show，点击可查看，主要来自pygments官方的Demo：

* [monokai](http://pygments.org/demo/1474764/?style=monokai)
* [manni](http://pygments.org/demo/1474764/?style=manni)
* [rrt](http://pygments.org/demo/1474764/?style=rrt)
* [perldoc](http://pygments.org/demo/1474764/?style=perldoc)
* [borland](http://pygments.org/demo/1474764/?style=borland)
* [colorful](http://pygments.org/demo/1474764/?style=colorful)
* [default](http://pygments.org/demo/1474764/?style=default)
* [murphy](http://pygments.org/demo/1474764/?style=murphy)
* [vs](http://pygments.org/demo/1474764/?style=vs)

等等，有兴趣的可以到[pygments Demo](http://pygments.org/demo)中查看.


###参考资料
* [http://pygments.org/]()
* [http://segmentfault.com/q/1010000000261050]()
* [http://havee.me/internet/2013-07/jekyll-install.html]()
* [http://havee.me/internet/2013-08/support-pygments-in-jekyll.html]()
* [http://segmentfault.com/a/1190000000661337]()