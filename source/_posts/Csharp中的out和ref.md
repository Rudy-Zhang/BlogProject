title: "C#中的out和ref"
date: 2014-03-04 21:27
tags: [C#]
category: [.NET]
---

##为什么要用out和ref
C#中默认的参数传递都是值传递，想要使用**引用传递**就需要使用ref和out关键字，ref和out都是引用传递，但是逻辑上有区别
##区别
ref需要初始化参数才能进行传递，逻辑是已经有了，传进来，来帮我改改

out不需要，out的逻辑是不管有没有，传进来，主要给我传出来

##例子

``` C++
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace study
{
    public class Program
    {
        // ref关键字可以让一个值类型的输入按引用传递，类似于C语言中的&
        private static void refFunction(int x,ref int a)  
        {  
            a = a * x;  
        }   
        // 使用out关键字,输入参数str在传递给outKeyWord()方法前不必进行初始化
        private static void outKeyWord(out string str)  
        {  
            //输出参数str在方法返回前必须被赋值 
            str = "a string";  
        }  
        static void Main(string[] args)  
        {  
            //***********ref关键字的使用  
            int x = 12, a = 5;  
            Console.WriteLine(a);//输出:5  
            refFunction(x, ref a);  
            Console.WriteLine(a);//输出:60  
              
            //***********out关键字的使用  
            string str;//不必初始化  
            outKeyWord(out str);//输出:a string  
            Console.WriteLine(str);  
            Console.ReadKey();  
  
        }  
            
    }
}
```