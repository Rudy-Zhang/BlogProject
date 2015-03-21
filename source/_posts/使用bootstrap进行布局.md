title: "使用bootstrap进行布局"
date: 2015-03-21 10:49:20
category: [前端]
tags: [bootstrap]
---
##前言
bootstrap来自twitter，是最受欢迎的前端框架。Bootstrap提供了优雅的HTML和CSS规范，它即是由动态CSS语言Less写成。
> LESS 包含一套自定义的语法及一个解析器，用户根据这些语法定义自己的样式规则，这些规则最终会通过解析器，编译生成对应的 CSS 文件。

[使用boostrap的组件](http://v3.bootcss.com/components/)
[寻找漂亮的的bootstrap主题](https://bootswatch.com/)
本文主要讨论bootstrap的布局

##栅格化布局
> Bootstrap 提供了一套**响应式**、**移动设备优先**的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。

##内边距，外边距
首先需要理解，html内边距（padding），外边距（margin）

CSS中的边距指的是当前元素border与周围其它元素border的距离（或者称为空间）。

**Box Model**: 任意一个块级元素均由
* content(内容),
* padding
* background(包括背景颜色和图片),
* border(边框)
* margin
 五个部分组成
![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqd4plmz36j20dk0crmyn.jpg)

##栅格系统之布局容器.container

Bootstrap 需要为页面内容和栅格系统包裹一个 .container 容器。我们提供了两个作此用处的类。
* .container 类用于固定宽度并支持响应式布局的容器。
* .container-fluid 类用于 100% 宽度，占据全部视口（viewport）的容器。

作用：
* 在响应式宽度上提供**宽度约束**。响应式尺寸的改变其实改变的是 container ，行 (rows) 和列 (columns) 都是基于1 、在响应式宽度上提供宽度约束。响应式尺寸的改变其实改变的是 container ，行 (rows) 和列 (columns) 都是基于百分比的所以它们不需要做任何改变；
* 提供 padding 以至于不内容不直为紧贴于浏览器边缘，两边都是 15px ，下图中粉色的就是了，稍后解释更多；
![enter image description here](http://ww4.sinaimg.cn/mw690/4c2edcb7jw1eqd4plxjvkj20go08kaad.jpg)
**在一个container中永远不需要再嵌一个container，记住永远不要。**

##栅格系统之row
row 为 col 提供了空间，理想上一行上分了 12 col
Rows  的两侧都拥有独特的负 15px margin 值 
**永远不要在 container 外用 row, row 在 container 外面使用是无效的**
![enter image description here](http://ww4.sinaimg.cn/mw690/4c2edcb7jw1eqd4pm6ym6j20go08jmxm.jpg)

##栅格系统之column
列(col)现在有15px的padding了，如下图中黄颜色部分。Container的正padding值造成了15px的留空，row用负margin值反的延伸回去，
**所以现在col的padding值与container的padding重叠了**
![enter image description here](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqd4pmjifqj20go08kjrz.jpg)
**永远不要在row外使用col,在row外使用col是无效的**
然后col就是一个.container了，你可以在里面继续使用row了
**container/row/column整这么些事儿最终要达到的结果就体现在这里了，就是为了col的补白**
