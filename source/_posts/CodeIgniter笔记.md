title: "CodeIgniter笔记"
date: 2014-10-28
tags: [CodeIgniter] 
category: [PHP]
---
###CodeIgniter
> CodeIgniter 是一个简单快速的PHP MVC框架。
> CodeIgniter 是一套给 PHP 网站开发者使用的应用程序开发框架和工具包。它提供一套丰富的标准库以及简单的接口和逻辑结构，其目的是使开发人员更快速地进行项目开发。使用 CodeIgniter可以减少代码的编写量，并将你的精力投入到项目的创造性开发上。

简单快速，这也是选择CI的原因。

以下内容细节可以从CI[官方文档](http://codeigniter.org.cn/user_guide/index.html)寻找

###超级对象this
当前的控制器对象，this，提供了很多属性
加载方法

```
$this->load->
```
装载器类的实例system/core/Loader.php
提供的方法：
- view()     装载视图
- vars()     分配变量到视图 
- database() 装载数据库操作
- model()   装载模型对象
- helper() 
###var_dump

```PHP
void var_dump ( mixed $expression [, mixed $... ] )
```
此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。

###$this->input 输入类
作用：默认帮助进行安全处理
CI_Input类 system/cor/Input.php
`$this->input->server('DOCUMENT_ROOT') `相当于`$_POST['DOCUMENT_ROOT']`
`$this->input->post('表单提交的name')`

###URL相关函数
- site_url
在form表单中提交action不能把路径写死,使用`site_url (controller/action)`
- base_url 根目录
请求图片时，`<img src="<?php echo base_url()?>uploads/jh.jpg">`
- 自动加载URL函数，autoload.php
$autoload['helper'] = array('url');

###路由
`config/routes.php`
默认的控制器

```
$route['default_controller']=
```
伪静态页面,不再显示`xxx.php`,在路由中写一个正则表达式

```
$route['show/([\d]+)\.html']='article/show/'
```
###分页
使用分页类`pagination`
####加载类文件

```
 $this->load->library('pagnination');
```
####配置 

```
$config['per_page']=10;//每页显示10条数据
```
其他配置见[文档](http://codeigniter.org.cn/user_guide/libraries/pagination.html)
####初始化 

```
$this->pagination->initialize($config)
```
####创建链接

```
$this->pagination->create_links($config)
```
####跳过多少条
 sql语句 `select from limit+跳过的条数`
 
####定制分页器
$config['first_link']="首页"
 其他配置见[文档](http://codeigniter.org.cn/user_guide/libraries/pagination.html)
 
###文件上传
表单
`<form enctype="multipart/form-data"></form>`
####使用文件上传类
```
 $this->load->library('')
```
####配置
见[文档](http://codeigniter.org.cn/user_guide/libraries/pagination.html)
####上传

```
 $this->upload->do_upload('pic')
```
####返回信息
文件上传的一些数据

```
 $his->upload->data()
```
###session
CI中的session 默认不使用php的原生session，**放在cookie中**（限制大小4k），不太好使用

```
$this->session->set_userdata('user',$user) ; //使用键值对来存储数据
```
**因为是存储在cookie中的，所以不能在当前的函数中获取到session中的数据，需要跳转的别的页面后才可以获取的到**

####加密
生成随机不重复的字符串作为加密用的key，保存到config.php

```
$['encryption_key']="key的值"
```
或者不使用CI的session，囧。。。

###表单验证
####使用表单验证类
$this->load->library('form_validation');
####验证

```
$this->form_validation->set_rules('','','');
$bool=$this->form_validation->run();
```
前台验证使用js，后台验证使用表单验证类
####输出错误
`form_error('name','<span>','</span>')`
`echo validation_error()`直接输出所有结果（推荐使用）




