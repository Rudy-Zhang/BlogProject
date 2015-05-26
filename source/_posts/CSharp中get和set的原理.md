title: "C#中get和set的原理"
date: 2014-02-26 11:44:00
category: [.NET]
tags: [C#]

---
##为啥要使用get，set？
软件工程的思想是用户只要指定你去干什么就好了，而不用关心你是怎么干的。所以如果直接声明一个public的变量，就能够在类外对变量进行各种操作，从而影响了类内部对变量的操作。
```
public class A
{
    public int Age;   //这是不好的，待会有程序员可能把-1赋给Age
}
```
为了避免这样乱搞，Java推荐用户对变量使用getValue(),和setValue方法，而C#嘛，进行一点微创新，使用property的get，set对类内的属性进行封装。**可以使用prop+tab的快捷键调出get，set**
##一种写法
```
public class A
{
    private int age;
    public int Age
    {
        get { return age; }
        set { age = value; }
    }
}
        
static void Main(string[] args)
{
    A a=new A();
    a.Age = 3;
    Console.WriteLine(a.Age);
}
```
这样类内只对age操作，类外只对Age操作，逻辑上是一个东西，这里的get{}实际上就是java中的getValue方法。

其实这只是一种编码风格，**类内部的变量用小写，暴露的Property首字母大写，变量不能public**

需要注意的是，age和Age仅仅是一种逻辑上的关系，**Age虽然是public，但是set和get规定了Age是否能够在类外读取和修改**

##推荐写法

在.NET自己的代码中没有小写的变量，**类内的变量一律使用属性即property表达。**

修饰属性使用public或者protect，这样一种变量就只有一个名字了。

同理，**public的变量使用set，get规定类外的读写权限。protect变量的set，get规定子类的读写权限。**

如下为Form类的写法：
![Form类的写法](http://ww4.sinaimg.cn/mw690/4c2edcb7jw1eqa9861zk5j20jr09iq45.jpg)
##体现封装性-------进行判断
get，set不只是能够返回和设置变量的值，进行只读，只写，读写三种操作，要不然这也太挫了
```
public class A
{ 
    private int age;
    public int Age;
    {
        get { return age; }
        set { 
            if(age>0)
            age = value; 
        }
    }
}
```
这样就可以防止类外对age的不合法赋值啦，类内部的逻辑完全在类内部执行，体现了OO的思想