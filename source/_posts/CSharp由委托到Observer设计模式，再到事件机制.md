title: "C#由委托到Observer设计模式，再到事件机制"
date: 2014-03-11 11:30:25
tags: [C#]
category: [.NET]

---

##什么是委托
处理诸如int，bool等基本数据类型，它们是数据的类型。**委托，是方法的类型**。如：
```
 int a; //a可以是1,2,3,4,5.........
```

那么
```C#
delegate D;//D相当于int,是一种类型
D d; //声明了变量，d可以是fun1,fun2,fun3........这里funX是函数的名字。
```

##如何使用委托
函数并不像数那么简单直接赋值就可以了，函数有返回值，或者无返回值，还有参数列表。

如果说**定义了一个委托，那么这个委托就对应了一种函数(返回值+参数列表)，像具有了int和double一样的类型。**

为了使得声明委托能够和一种函数对应，我们使用这样的方法声明委托:

```
public delegate void MyDelegate(int a);//委托的声明，public规定了delegate的可见性
public static void myFunction(int a) //普通方法的声明
{
    Console.WriteLine("a=" + a);
}
```
这样，MyDelegate就对应了一种函数，相当一种函数的类型(对应于数据类型的int，double)。那么使用这个类型就可以声明变量了

```
static void Main(string[] args)
{
    MyDelegate d;
    d = myFunction;//将myFunction绑定到d，因为具有相同的返回值和参数列表，所以可以绑定
}
```
###委托的绑定

委托不同于普通数据类型(如int)的一个特性是：可以将多个方法赋给同一个委托，或者叫**将多个方法绑定到同一个委托，当调用这个委托的时候，将依次调用其所绑定的方法。**

看下面的例子：

```
class Program
{
    public delegate void MyDelegate(int a);//委托的声明，public规定了delegate的可见性
    public static void myFunction1(int a) //普通方法1的声明
    {
        Console.WriteLine("a=" + a);
    }
    public static void myFunction2(int a) //普通方法2的声明
    {
        Console.WriteLine("a等于：" + a);
    }
    static void Main(string[] args)
    {
        MyDelegate d;
        d = myFunction1;
        d += myFunction2;
        d(3);//调用这个委托
        Console.ReadLine();
    }
}
```
###总结
委托是一个类，它定义了**方法的类型**，使得可以将方法**当作另一个方法的参数来进行传递**，这种将方法动态地赋给参数的做法，可以避免在程序中大量使用If-Else(Switch)语句，同时使得程序具有更好的可扩展性。

##观察者模式（Observber）
事件处理机制的基础是一种叫观察者模式的设计模式。Observer设计模式是为了定义对象间的一种**一对多**的依赖关系，以便于当一个对象的状态改变时，其他依赖于它的对象会被**自动告知并更新**。Observer模式是一种松耦合的设计模式。

举个简单的例子：
> 假设我们有个高档的热水器，我们给它通上电，当水温超过95度的时候：1、扬声器会开始发出语音，告诉你水的温度；2、液晶屏也会改变水温的显示，来提示水已经快烧开了。

现在我们需要写个程序来模拟这个烧水的过程，我们将定义一个类来代表热水器，我们管它叫：**Heater**，它有代表水温的字段，叫做**temperature**；当然，还有必不可少的给水加热方法**BoilWater()**，一个发出语音警报的方法**MakeAlert()**，一个显示水温的方法，**ShowMsg()**。

在本例中，事情发生的顺序应该是这样的：

1. 警报器和显示器告诉热水器，它对它的温度比较感兴趣(注册)。
2. 热水器知道后保留对警报器和显示器的引用。
3. 热水器进行烧水这一动作，当水温超过95度时，通过对警报器和显示器的引用，自动调用警报器的MakeAlert()方法、显示器的ShowMsg()方法。

在这个设计模式中有两种角色：
1. Subject：监视对象，它往往包含着其他对象所感兴趣的内容。在本范例中，热水器就是一个监视对象，它包含的其他对象所感兴趣的内容，就是temprature字段，当这个字段的值快到100时，会不断把数据发给监视它的对象。
2. Observer：监视者，它监视Subject，当Subject中的某件事发生的时候，会告知Observer，而Observer则会采取相应的行动。在本范例中，Observer有警报器和显示器，它们采取的行动分别是发出警报和显示水温。

代码如下：

```
namespace Observer
{
    public interface Iobservable //subject对象具有的接口，所有subject都要继承于这个接口
    {
        void add(Iobserver o);
        void notify();

    }
    
    public interface Iobserver //observer对象具有的接口，所有observer都要继承于这个接口
    {
        void update(int temperature);
    }

    public class Heater : Iobservable
    {
        private int temperature;
        List<Iobserver> list = new List<Iobserver>();
        // 烧水
        public void BoilWater()
        {
            for (int i = 0; i <= 100; i++)
            {
                temperature = i;

                if (temperature > 95)
                {
                    notify();
                }
            }
        }
        public void add(Iobserver o)
        {
            list.Add(o);
        }

        public void notify()
        {
            foreach (var o in list)
            {
                o.update(temperature);
            }
        }
    }

    // 警报器
    public class Alarm : Iobserver
    {
        public void MakeAlert(int param)
        {
            Console.WriteLine("Alarm：嘀嘀嘀，水已经 {0} 度了：", param);
        }

        public void update(int temperature)
        {
            MakeAlert(temperature);
        }
    }

    // 显示器
    public class Display : Iobserver
    {
        public static void ShowMsg(int param)
        { //静态方法
            Console.WriteLine("Display：水快烧开了，当前温度：{0}度。", param);
        }

        public void update(int temperature)
        {
            ShowMsg(temperature);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Heater h = new Heater();
            Alarm a = new Alarm();
            Display d = new Display();
            h.add(a);//注册
            h.add(d);//注册
            h.BoilWater();
            Console.ReadLine();
        }
    }
}
```
输出：
> Alarm：嘀嘀嘀，水已经 96 度了：
Display：水快烧开了，当前温度：96度。
Alarm：嘀嘀嘀，水已经 97 度了：
Display：水快烧开了，当前温度：97度。
Alarm：嘀嘀嘀，水已经 98 度了：
Display：水快烧开了，当前温度：98度。
Alarm：嘀嘀嘀，水已经 99 度了：
Display：水快烧开了，当前温度：99度。
Alarm：嘀嘀嘀，水已经 100 度了：
Display：水快烧开了，当前温度：100度。

例子中的observable就是subject，Heater继承这个接口。Alarm和Display是观察者，继承observer接口。Iobserverable可以添加观察者(注册)，可以通知观察者。Iobserver可以更新自己的信息。这样就完成了观察者的注册观察，以及通知的过程。

通过以上过程可以发现，**注册这个行为不就是委托当中的注册吗？通知这个行为不就是委托的调用吗？水烧开这不就是一个事件的发生吗？**

##C#中的事件处理机制
###委托声明成private还是public？

因为声明委托的目的就是为了把它暴露在类的客户端进行方法的注册，你把它声明为private了，客户端对它**根本就不可见**，那它还有什么用？但是如果声明为public在客户端可以对它进行随意的赋值等操作，**严重破坏对象的封装性**。这时候就轮到**event关键字**出场了
###event关键字有什么作用？
前面提到委托在程序运行事会变成一个类，如果使用了event关键字，又会生成一个类，将前面的类**封装起来**，这样做的作用是：

**在类的内部的delegate，不管你声明它是public还是protected，它总是private的。在类的外部，注册“+=”和注销“-=”的访问限定符与你在声明事件时使用的访问符相同。**

猜测实现方法类似于：

```
class event{
     private Delegate delegate;
     重载+=（）
     {
          delegate...
     }
}
```
如果使用事件处理的方法实现observer的例子，代码如下：

```
using System;
using System.Collections.Generic;
using System.Text;

namespace Delegate {
    // 热水器
    public class Heater {
       private int temperature;
       public delegate void BoilHandler(int param);   //声明委托
       public event BoilHandler BoilEvent;        //<strong>声明事件！！！！！！！！！！！！！！！！！！！！！！！！！！！</strong>

       // 烧水
       public void BoilWater() {
           for (int i = 0; i <= 100; i++) {
              temperature = i;

              if (temperature > 95) {
                  if (BoilEvent != null) { //如果有对象注册
                      BoilEvent(temperature);  //调用所有注册对象的方法
                  }
              }
           }
       }
    }

    // 警报器
    public class Alarm {
       public void MakeAlert(int param) {
           Console.WriteLine("Alarm：嘀嘀嘀，水已经 {0} 度了：", param);
       }
    }

    // 显示器
    public class Display {
       public static void ShowMsg(int param) { //静态方法
           Console.WriteLine("Display：水快烧开了，当前温度：{0}度。", param);
       }
    }
   
    class Program {
       static void Main() {
           Heater heater = new Heater();
           Alarm alarm = new Alarm();

           heater.BoilEvent += alarm.MakeAlert;    //注册方法
           heater.BoilEvent += (new Alarm()).MakeAlert;   //给匿名对象注册方法
           heater.BoilEvent += Display.ShowMsg;       //注册静态方法

           heater.BoilWater();   //烧水，会自动调用注册过对象的方法
       }
    }
}
```
输出为：
> Alarm：嘀嘀嘀，水已经 96 度了：
Alarm：嘀嘀嘀，水已经 96 度了：
Display：水快烧开了，当前温度：96度。
// 省略...

###平时处理事件会有Object o,... eventargs？
其实这是 **.Net Framework的编码规范**：
> *  委托类型的名称都应该以EventHandler结束。那么就应该这样声明
> * 委托的原型定义：有一个void返回值，并接受两个输入参数：一个Object 类型，一个 EventArgs类型(或继承自EventArgs)。
> * 事件的命名为 委托去掉 EventHandler之后剩余的部分。
> * 继承自EventArgs的类型应该以EventArgs结尾。

所以声明应该变成这样：

```
public delegate void BoiledEventHandler(Object sender, BoiledEventArgs e); //声明委托
public event BoiledEventHandler Boiled; //声明事件
```
烧水的函数就变成这样：

```
// 烧水。
public void BoilWater() {
   for (int i = 0; i <= 100; i++) {
      temperature = i;
      if (temperature > 95) {
          //建立BoiledEventArgs 对象。
          BoiledEventArgs e = new BoiledEventArgs(temperature);
          OnBoiled(e);  // 调用 OnBolied方法
      }
   }
}
```
委托声明原型中的**Object类型的参数代表了Subject，也就是监视对象**，在本例中是 Heater(热水器)。回调函数(比如Alarm的MakeAlert)可以通过它访问触发事件的对象(Heater)。**EventArgs 对象包含了Observer所感兴趣的数据，在本例中是temperature。**

完整实例如下，供参考：

```
using System;
using System.Collections.Generic;
using System.Text;

namespace Delegate {
    // 热水器
    public class Heater {
       private int temperature;
       public string type = "RealFire 001";       // 添加型号作为演示
       public string area = "China Xian";         // 添加产地作为演示
       //声明委托
       public delegate void BoiledEventHandler(Object sender, BoiledEventArgs e);
       public event BoiledEventHandler Boiled; //声明事件

       // 定义BoiledEventArgs类，传递给Observer所感兴趣的信息
       public class BoiledEventArgs : EventArgs {
           public readonly int temperature;
           public BoiledEventArgs(int temperature) {
              this.temperature = temperature;
           }
       }

       // 可以供继承自 Heater 的类重写，以便继承类拒绝其他对象对它的监视
       protected virtual void OnBoiled(BoiledEventArgs e) {
           if (Boiled != null) { // 如果有对象注册
              Boiled(this, e);  // 调用所有注册对象的方法，事件发生
           }
       }
      
       // 烧水。
       public void BoilWater() {
           for (int i = 0; i <= 100; i++) {
              temperature = i;
              if (temperature > 95) {
                  //建立BoiledEventArgs 对象。
                  BoiledEventArgs e = new BoiledEventArgs(temperature);
                  OnBoiled(e);  // 调用 OnBolied方法
              }
           }
       }
    }

    // 警报器
    public class Alarm {
       public void MakeAlert(Object sender, Heater.BoiledEventArgs e) {//使得类型符合.net事件规范，满足委托类型，可以注册
           Heater heater = (Heater)sender;     //这里是不是很熟悉呢？
           //访问 sender 中的公共字段
           Console.WriteLine("Alarm：{0} - {1}: ", heater.area, heater.type);
           Console.WriteLine("Alarm: 嘀嘀嘀，水已经 {0} 度了：", e.temperature);
           Console.WriteLine();
       }
    }

    // 显示器
    public class Display {
       public static void ShowMsg(Object sender, Heater.BoiledEventArgs e) {   //静态方法
           Heater heater = (Heater)sender;
           Console.WriteLine("Display：{0} - {1}: ", heater.area, heater.type);
           Console.WriteLine("Display：水快烧开了，当前温度：{0}度。", e.temperature);
           Console.WriteLine();
       }
    }

    class Program {
       static void Main() {
           Heater heater = new Heater();
           Alarm alarm = new Alarm();

           heater.Boiled += alarm.MakeAlert;   //注册方法
           heater.Boiled += (new Alarm()).MakeAlert;      //给匿名对象注册方法
           heater.Boiled += new Heater.BoiledEventHandler(alarm.MakeAlert);    //也可以这么注册
           heater.Boiled += Display.ShowMsg;       //注册静态方法

           heater.BoilWater();   //烧水，会自动调用注册过对象的方法
       }
    }
}


```
输出为：
> Alarm：China Xian - RealFire 001:
Alarm: 嘀嘀嘀，水已经 96 度了：
Alarm：China Xian - RealFire 001:
Alarm: 嘀嘀嘀，水已经 96 度了：
Alarm：China Xian - RealFire 001:
Alarm: 嘀嘀嘀，水已经 96 度了：
Display：China Xian - RealFire 001:
Display：水快烧开了，当前温度：96度。
// 省略 ...