title: "从Lambda表达式到Linq查询"
date: 2014-03-27 11:52
category: [.NET]
tags: [C#]

---
##Lambda表达式
lamda表达式是一种匿名函数（没有函数名），使用方式
> 输入参数=>表达式或语句块

仅当 lambda 只有一个输入参数时，括号才是可选的；否则括号是必需的。例：

```
(x, y) => x == y
```
显示指定类型

```
(int x, string s) => s.Length > x
```

异步lamda：

```
button1.Click += async (sender, e) =>
{
    textBox1.Text += "\r\nControl returned to Click event handler.\r\n";
};
```

Lamda中的**类型推理**：
通过推断来确定lamda表达式的输入参数的类型和返回值
Lambda 的一般规则如下：
-  Lambda 包含的参数数量必须与委托类型包含的参数数量相同。
- Lambda 中的每个输入参数必须都能够隐式转换为其对应的委托参数。
- Lambda 的返回值（如果有）必须能够隐式转换为委托的返回类型。

举例：

```
customers.Where(c => c.City == "London");
```

通过c.City == "London"来推断出最终c的类型。
##Linq
Linq是**使用C#语法进行的强类型查询语言**，目的是提供统一对称的方式，在广义的数据上得到数据和操作数据.

好处：可以进行多种数据形式的查询，如SQL，XML
Linq应用场景：
- Linq to object 针对数组和集合
- Linq to XML 操纵查询XML文档
- Linq to DataSet 针对ADO.Net的DataSet对象
- Linq to Entities：针对ADO.Net Entity Framework API使用LINQ查询

举例：Linq to object

```
 string[] strs = { "Morrroewed", "djfuajre 2" , "dfa ad f 3", "ad adf 4" };
IEnumerable<string > subset = from str in strs
    where str.Contains(" " )
    orderby str
    select str;
    foreach (string s in subset)
        Console.WriteLine("item:{0}" , s);
```
结果集由实现了IEnumberable<T>的对象表示，在这个例子中T为string
实际上查询的函数都是由IEnumberable<T>中的函数（被子类重载后）来帮助我们实现的

简便方法：**使用隐式类型**

```
var subset = from str in strs
```
Linq的操作只有在迭代查询结果的时候才执行，这种功能叫做**延迟执行**
Linq中关键词的作用：
- from in指定查询数据容器,
- where 查询条件
- select 从容其中选择一个序列
- join、on、equals、into基于指定键进行关联操作
- order by、ascending、descending 排序
- group by 对数据分组

转换结果集：
- Reverse<>()
- RoArray<>()
- ToList<>()

from item in container,select item
item是从container中查找的元素，可以起任意的名字，select是最终选择的内容

##使用Enumerable类型和Lamda表达式建立查询
###原理
Linq to object中的一些功能可以使用Enumerable对象（可以理解为数据容器）的扩展方法来访问。
如调用Enumerable.Where()方法

```
public static IEnumerable<Tsource> Where <Tsource>(this IEnumerable<Tsource> source,System.Func<Tsource,int,bool> predicate)
```

其中Func<>是一个**泛型委托**，接受一定的参数，通过把写好的**匿名函数**（Lamda表达式）作为参数传入，就可以执行查询逻辑了
###目的
使得查询语句写法更加简便

###举例

```
var subset = strs.Where(str => str.Contains( " "));
```
str相当于from str in strs
 str.Contains( " "))相当于执行where的条件判断
 
###更复杂的版本
var subset = strs.Where(str => str.Contains( " ")).OrderBy(str=>str).Select(str=>str);

###后记
这种方法不过是LINQ查询语句的简记版本而已