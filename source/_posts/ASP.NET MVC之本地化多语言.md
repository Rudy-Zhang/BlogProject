title: "ASP.NET MVC之本地化多语言"
date: 2015-03-20 19:56:58
category: [.NET]
tags: [ASP MVC]

---

##前言
使用ASP MVC的本地化多语言解决方案可以方便地让我们把程序中的字符串映射到一个资源文件中，这样便于统一修改，同时只要指定使用不同的语言，就可以瞬间切换资源文件，从而达到灵活修改网站的目的。
##新建资源文件
如图在项目中新建了三个资源文件，一个默认Global.resx,一个是英文Global.en.resx，一个是备份的Global.ar.resx
![enter image description here](http://ww3.sinaimg.cn/mw690/4c2edcb7jw1eqcfrko5gaj209u07rdg5.jpg)

##修改key-value
![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqcfrkznssj20k70cidhq.jpg)


##什么时候使用
ASP.NET MVC 还提供将强类型数据或对象传递到视图模板的能力。便于检查类型错误。
使用方法：
- 简单独立的数据使用ViewBag
- 向前端传递一个大的对象，使用试图模板，便于检查错误

##如何把Key放入到程序当中
###在Model中
对应Display属性使用

```
[Display(Name = "UserName",ResourceType=typeof(Resources.Global))]
```
对于ErrorMessage

```
[Required(ErrorMessageResourceName = "PasswordRequiredMessage", ErrorMessageResourceType = typeof(Resources.Global))]
```

###在Controller中

一般来说在Controller不出现汉字的字符串，如果使用，可以通过如下方法

```
string myString=HospitalProject.Resources.Global.DoctorName;
```
###在前端
使用razor表达式
```
@Html.ActionLink(@HospitalProject.Resources.Global.NetworkDiagnosisHome, "Index", "Home", routeValues: null, htmlAttributes: new { @class = "navbar-brand" })
```
##如何指定资源文件
在`ViewStart.cshtml`中：

```
XXXProject.Extensions.CultureHelper.SetCulture("ar");
```
##后记
通过本方法可以方便进行网站资源文件的切换，如果需要实现网站同时支持多种语言，使用cookie或者URL route需要进行其他配置。参考：
[参考1](http://jingyan.baidu.com/article/3052f5a1d4410797f31f86f8.html)
[参考2](http://www.cnblogs.com/xyfy/articles/1970424.html)
[参考3](http://afana.me/post/aspnet-mvc-internationalization.aspx)