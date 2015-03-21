title: "ASP.NET MVC 之Razor"
date: 2014-10-10 10:38
category: [.NET]
tags: [ASP MVC]
---

##什么是razor？
Razor类似于JSP是一种服务端标记语言，运行的时候被程序动态转化为Html，用户只能看到相应的html文件。

##简单的语法
简单直接的理念，@后直接写代码：
- 代码 ：@{此处为Razor代码}
- 在html中使用@变量名，来引用asp.net对象
- Razor通过从尖括号会看来确定变量，但有时会出现二义性，就需要用括号解决@(rootNamespace).Models
- @@转义一个@字符

使用 var 关键词或类型对变量进行声明，不过 ASP.NET 通常能够自动确定数据的类型。

##razor组织页面的结构
1. View下的_ViewStart.cshtml优先于任何代码的运行
2. 通过这个页面指定页面布局@{layout=........}
3. 在渲染页面的时候对layout属性重写，从而选择一个新的布局
