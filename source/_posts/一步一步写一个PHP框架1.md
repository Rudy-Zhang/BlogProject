title: "一步一步写一个PHP框架（一）"
date: 2015-04-20 15:31:44
category: [PHP]
tags: []
---

##前言

完成了一个小项目，使用Apache+MySql+PHP。PHP中使用的框架是CodeIgniter，这是一个轻量易扩展的PHP MVC框架，适用于轻量级的网站搭建。CodeIgniter手册清晰丰富，覆盖了一般网站的所有需求。

为了更好地理解PHP里面框架的设计，目标是自己写一个简单的PHP MVC框架，一方面可以更深入地学习PHP，一方面也能够更深入地理解框架设计里面的思想。

##Apache的工作流程

Apache是一个Web服务器，也可以叫做http服务器，因为Apache只能处理http请求。常见的web服务器还有Nginx

Apache的工作流程如下：

1. 浏览器向服务器发出HTTP请求(Request)。
2. 服务器收到浏览器的请求数据，经过分析处理，向浏览器输出响应数据（Response）。
3. 浏览器收到服务器的响应数据，经过分析处理，将最终结果显示在浏览器中。

Apache和Nginx都属于Web服务器，两者都实现了HTTP 1.1协议。
 
所以如果有人问FTP协议可以在Apache上工作吗？答案是用Apache FTP Server可以，Apache web服务器不可以。

Apache是用C语言写的，服务器当然要追求高效率。

##PHP原理

PHP的核心解释器是用C语言写的，相当于我们写了PHP代码，会有一个C语言写好的解释工具一边解释，一边执行。实际上在写PHP的时候就是在使用一个C语言写好的工具，我们的PHP代码就是指挥这个工具的命令。

PHP包括四层体系：

1. Zend引擎：Zend整体用纯C实现，是PHP的内核部分。负责翻译PHP代码，是一切的核心。
2. Extensions：围绕Zend引擎，通过组件方式提供基础服务。常见内置函数如array是由extension实现的。
3. Sapi通过一系列的**钩子函数**，使得PHP可以和外围交互数据。如Apache
4. 上层应用：平时写的PHP程序

>如果PHP是一辆车，那么车的框架就是PHP本身，Zend是车的引擎（发动机），Ext下面的各种组件就是车的轮子，Sapi可以看做是公路， 车可以跑在不同类型的公路上，而一次PHP程序的执行就是汽车跑在公路上。因此，我们需要：性能优异的引擎+合适的车轮+正确的跑道。

PHP的执行的核心是翻译出来的一条一条指令，也即opcode。

**hashtable**是PHP的核心数据结构，数组就是典型的应用。Zend hash table实现了典型的hash表散列结构，同时通过附加一个双向链表（解决冲突），提供了正向、反向遍历数组的功能。

更详细的内容参考[这里](!http://www.cnblogs.com/zcy_soft/archive/2013/03/14/2959396.html)

##PHP在Apache上运行

###以模块加载的方式运行

这种方式使用了Apache的Hook机制。所谓Hook机制，就是在自己的程序运行的时候允许别的模块插上一腿。当我们配置Apache服务器的`http.config`文件时，写入`mod_php5.so/php5apache2.dll`就是将自定义的函数注入到Apache的请求处理循环当中。在模块化的运行方式中，PHP与Web服务器一起启动并且运行，通过Apache自身的进程线程管理来处理并发的请求。

###以CGI，FastCGI方式运行

CGI英文叫做公共网关接口，CGI是外部应用程序（CGI程序）与Web服务器之间的接口标准，是在CGI程序和Web服务器之间传递信息的规程。Apache在Http请求的时候会将请求提交给CGI应用程序（php-cgi.exe）解释，解释之后的结果返回给Apache，然后再返回给相应的请求用户。

CGI VS FastCGI

FastCGI是CGI的加强版本，CGI是单进程，多线程的运行方式，程序执行完成之后就会销毁。FastCGI是常驻(long-live)型的CGI.有自身的进程管理器，不必每一次都fork一个进程（CGI解释器）去执行。常见的PHP-FPM是一个PHP FastCGI管理器。

关于更多的对比可以看[这里](http://fifiole.blog.163.com/blog/static/169459225201222962651804/)。


> 目前在HTTPServer这块基本可以看到有三种stack比较流行：
> 

> （1）Apache+mod_php5



> （2）lighttp+spawn-fcgi



> （3）nginx+PHP-FPM



> 三者后两者性能可能稍优，但是Apache由于有丰富的模块和功能，目前来说仍旧是老大。有人测试nginx+PHP-FPM在高并发情况下可能会达到Apache+mod_php5的5~10倍，现在nginx+PHP-FPM使用的人越来越多。



