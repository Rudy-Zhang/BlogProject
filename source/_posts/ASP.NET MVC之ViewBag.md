title: "ASP.NET MVC之ViewBag"
date: 2014-10-10 
category: [.NET]
tags: [ASP MVC]
---

##解释
ViewBag是一个动态对象，可以将属性添加到 ViewBag对象。
ViewBag是一个动态对象，这意味着可以随便提取这个属性 。
ViewBag是一个动态对象，提供方便的后期绑定方式将信息传递给视图。

##什么时候使用
ASP.NET MVC 还提供将强类型数据或对象传递到视图模板的能力。便于检查类型错误。
使用方法：
- 简单独立的数据使用ViewBag
- 向前端传递一个大的对象，使用试图模板，便于检查错误
##ViewBag VS ViewData VS TempData
- viewbag是动态的viewdata,是一种更受欢迎的语法
- ViewBag其实本质就是ViewData,只是多了层Dynamic控制。使用何种方式完全取决于个人的爱好。
- ViewBag和ViewData仅针对当前Action中有效，生命周期和View相同。
- TempData 只是一个容器（类似于ViewBag），区别在于，重定向后（下一个页面）仍然可用



