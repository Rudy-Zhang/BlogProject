title: "ASP.NET MVC之Entity Framewrok"
date: 2015-03-20 16:55:10
tags: [ASP MVC]
category: [.NET]
---

##解释
Entity Framework 是微软以 ADO.NET 为基础所发展出来的对象关系对应 (O/R Mapping) 解决方案。
> 对象关系映射（Object Relational Mapping，简称ORM）是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。

##三种方式

- Database-first（数据库先行）：从一个数据库开始，然后实体框架生成相应代码。
- Model-first（模型先行）：先从一个可视化模型开始，然后实体框架生成数据库和代码。
- Code-first（代码先行）：先从代码开始，然后实体框架生成数据库

CodeFirst 用中文说是代码优先，此技术可以让我们先写代码，然后由Entity Framework根据我们的代码建立数据库。

CodeFirst和ModelFirst的数据库连接方式不一样，但都可以现在VS中测试连接

##why codefirst？
代码优先的开发支持更加优美的开发流程，它允许你：
>* 在不使用设计器或者定义一个 XML 映射文件的情况下进行开发。
>* 允许编写简单的模型对象POCO (plain old classes)，而不需要基类。
>* 通过"约定优于配置"，使得数据库持久层不需要任何的配置
>* 也可以覆盖"约定优于配置"，通过流畅的 API 来完全定制持层的映射。

Code First是基于Entity Framework的新的开发模式，原先只有Database First和Model First两种。Code First顾名思义，就是先用C#/VB.NET的类定义好你的领域模型，然后用这些类映射到现有的数据库或者产生新的数据库结构。**写好代码就行，数据库几乎不需要操作**。

##使用codefirst建立数据库
###建立实体类

```
public class Student
{
    public int StudentID { get; set; }
    public string LastName { get; set; }
    public string FirstNidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
}
```
###DAL数据库连接层，创建实体集
```
public class SchoolContext : DbContext
{
    public DbSet <Student> Students { get; set ; }
    public DbSet <Enrollment> Enrollments { get; set ; }
    public DbSet <Course> Courses { get; set ; }
}
```
继承自System.Data.Entity下的DbContext，它将我们的数据模型映射到数据库中，将代码中的数据持久化。
**这里相当于做了代码和数据库的表映射。**
###向web.config中添加数据库连接

```
<add name=" SchoolContext" connectionString=" Data Source=(LocalDb)\v11.0;Initial Catalog=ContosoUniversity;Integrated Security=SSPI;AttachDBFilename=|DataDirectory|\ContosoUniversity.mdf" providerName=" System.Data.SqlClient" />
```
###总结
当系统开始运行，会自动检查SchoolContext 对应的数据库是否建立，如果没有建立就通过已经做好的实体类和表映射关系在数据库中映射出需要的数据库表。



##基于标注的前后端验证
在建立网站的时候，都需要进行数据验证。前后端都要验证，前端js，后台使用后台的代码。实际的项目中一般前后端程序员约定好，然后分别写代码进行验证，很麻烦。ASP MVC为我们建立了一套非常简单的方法，只需要为属性添加标注，即可在前后端使用同一套验证逻辑。

###添加标注
在已经建好的实体类中，可以为属性添加Data Annotations（标注）。

```
public class Movie {
    public int ID { get; set; }

    [Required]
    public string Title { get; set; }

    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }

    [Required]
    public string Genre { get; set; }

    [Range(1, 100)]
    [DataType(DataType.Currency)]
    public decimal Price { get; set; }

    [StringLength(5)]
    public string Rating { get; set; }
}
```
###后端验证
使用`ModelState.IsValid` 即可实现

```
[HttpPost]
public ActionResult Index(Party party)
{
    if(ModelState.IsValid)
    {
        // TO DO: save the party to database
        return View("Thanks");
    }
    return View();
}
```
###前端验证
打开配置文件`web.config`，配置前端验证开启

```
<appSettings>
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true"/>
</appSettings>
```
包含JQuery验证库

```
<script src="@Url.Content("~/Scripts/jquery-1.5.1.min.js")" type="text/javascript"></script>
<script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"></script>
<script src="@Url.Content("~/Scripts/jquery.validate.unobtrusive.min.js")" type="text/javascript"></script>

```
在前端输出验证信息

```
@Html.ValidationMessageFor()
```

ASP MVC为我们提供了可以满足大部分需求的验证逻辑。但是如果想定制验证逻辑，也是可以的。参考[自定义验证](http://prideparrot.com/blog/archive/2012/4/model_validation_in_asp_net_mvc)
##codefirst更新数据库
###启用迁移
打开VS，工具>库程序包管理器>程序包管理器控制台。使用命令
`Enable-Migrations`

###生成和运行迁移
- `Add-Migration`      对比当前数据库和模型的差异，生成相应的代码，使数据库和模型匹配的。
- `Update-Database`  将任何挂起的迁移到数据库。