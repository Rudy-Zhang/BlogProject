title: "ASP.NET MVC之URL映射"
date: 2014-10-10 
category: [.NET]
tags: [ASP MVC]
---

##通常
web 服务器通常会将进入的 URL 请求直接映射到服务器上的磁盘文件。例如：某个 URL 请求（比如 "http://www.w3school.com.cn/index.asp"）将映射到服务器根目录上的文件 "index.asp"。

##MVC的URL映射
MVC 框架的映射方式有所不同。MVC 将 URL 映射到方法。这些方法在类中被称为“控制器”。控制器负责处理进入的请求、处理输入、保存数据、并把响应发送回客户端。

ASP.NET MVC 所使用的默认 URL的路径映射为方法的调用：/[Controller]/[ActionName]/[Parameters]

##例子
传参数  `/HelloWorld/Welcome?name=Scott&numtimes=4 `

```
public string Welcome(string name, int numTimes = 1) {
     return HttpUtility.HtmlEncode("Hello " + name + ", NumTimes is: " + numTimes);}
