title: Effective C++ 读书笔记
date: 2015-08-21 07:23:33
category: [编程语言]
tags: [C++]
---


##条款1，视C++为一个语言联邦

可以把C++看成四个组成部分：

- C语言的部分 
- Object Oriented C++ 继承封装多态
- Template C++ 使用模板编程
- STL

每一个部分都有各自的规约

##条款2， 尽量以const，enum，inline替换#define

- \#define只有替换功能，在预处理阶段完成，没有**类型检查**，也没有封装性
- 使用const替代变量定义，inline替代函数定义
- 预处理器中，#include必不可少，#ifdef，#else可以用来进行控制编译

##条款3，尽可能使用const

只要是事实，就把它说出来。只要是const就要声明为const类型。

- const修饰变量
const char *p = greeting等价于char const *p = greeting 
char * const p = greeting 指针不可更改指向对象
- const修饰函数，是最有威力的应用

(1) const 返回值

(2) const 函数参数，使用最多

(3) const 成员函数，表明这个函数不能修改任何成员变量（static变量可以修改），也不能调用任何非const成员

补充，
volidate int a，告诉编译器这个值可能被未知因素修改，每次都要从内存中重新读取
mutable int a，可以突破const成员函数限制，在函数中被修改

##条款4，确定对象被使用前已先被初始化

- 成员初始化应该在构造函数之前，意味着要使用**成员初始化列表**进行成员变量的初始化
说明：成员变量总是以声明的次序被初始化

- 对于static变量，使用Singleton+inline，保证在对象使用前初始化

## 条款5，了解C++默默编写并调用了哪些函数

构造函数，拷贝构造函数，赋值函数，析构函数

##条款6，若不想使用编译器自动生成的函数，就该明确拒绝

- 如果某些对象不可复制（不能使用copy constructor）
不是很安全的做法：把拷贝构造函数声明为private
更好的做法：写一个**UnCopyable基类**，copy constructor声明为private

##条款7，为多态基类声明virtual析构函数

	class B:A{}
	A *b=new B()
	delete b
因为b是A类型的指针，所以会导致局部销毁（只有A的部分被销毁）

原则：

- **企图作为（多态的）base class的类理论上都应该有virtual函数**，否则不应该作为base class（虚指针会额外增加空间）
- 任何带有virtual 函数的类都应该把析构函数声明为virtual
不要试图继承任何STL容器，因为他们没有virtual的析构函数


##条款8，别让异常逃离析构函数

- 析构函数不能抛出异常，否则会导致不明行为。
- 析构函数应该吞下这个异常，防止传播
- **调用一个自己的函数，使得用户有机会来处理这个异常**。 

##条款9， 绝不要在构造或者析构过程中调用virtual函数


- 构造过程

		class A{
		public:     
			A(){
			     virtual fun()
			}
		}
		class B:A{}
		B b;

构造B->构造A->调用fun(),这时B还没构造完（被编译器看成A对象），导致virtual 函数不会下降到子类执行。

- 析构过程
析构B->析构A->调用fun()，这时B已经被析构掉了，同样virtual函数不会下降，得不到想要的结果。

##条款10，令operator= 返回一个reference to *this

为了保证连续运算如：A=B=C 相当于A = (B = C)
返回一个引用，不会调用copy constructor
对于+=同样适用

##条款11，在operator= 中处理自我赋值

判断一下，if (this == &rhs) return *this

##条款12，复制对象时勿忘其每一个成分

可能出现的问题

（1）对象中的非内置类型不能得到赋值

（2）对象从父类继承而来的变量不能得到赋值

- 赋值所有local成员（内置类型，对象）
- 调用所有base class中的适当的copy constructor

##条款13，以对象管理资源

C++申请释放的资源：堆内存（最常用），文件，互斥锁，数据库连接等。一旦申请资源，就必须释放，否则就会造成内存泄露。

以对象管理资源相当于，使用一个类（RAII类）封装这个资源，在构造时初始化，在析构时释放。声明这个对象时使用栈内存声明。

常用：

`auto_ptr` ,封装对象，重写了指针行为，看起来像一个指针。只能指向一个对象。复制或者赋值，会删除原来的指针。

`shared_ptr`，类似于auto_ptr，不过允许多个指针指向同一个对象，内部提供引用计数。
这两个是最常见的RAII类，在构造时初始化，析构时delete。（注意不能`auto_ptr`(new std::string[10])数组对象）

##条款14，在资源管理类中小心copying行为

类似于`auto_ptr`或者`shared_ptr`的处理方式，对于复制。可以：

- 禁止复制
- 引用计数，类似于shared_ptr

##条款15，在资源管理类中提供对原始资源的访问

隐式：如`auto_ptr`重写了指针行为，*ptr,`ptr->`使得这个变量看起来像一个指针。从而可以访问封装的资源

显示：提供get()函数返回资源

##条款16，使用new和delete时要采用相同的形式

A *a=new A() ,释放时 使用delete a

int *a=new a[100],释放时使用delete []a

##条款17，以独立语句将newed对象置入智能指针

	std::tr1::shared_ptr<Widget> pw(new Widget)
	processWidget(pwd, priority())

使用单独语句，不要放到一起可能会造成编译先后导致指针丢失。
其实不是很明白这点

##条款18，让接口容易被使用，不易被误用

- 导入新类型

		Date(int month, int day, int year)

多个参数，使用Month，Day，Year类型，可以预防接口被误用

- 接口一致性

如:stl每个容器都有size()方法

##条款19，设计class犹如设计type

设计一个类时需要考虑很多问题：

1. 创建和销毁
2. 初始化（初始化列表），拷贝构造函数
3. pass by value && pass by reference
4. 继承关系
5. 类型转换
6. 操作符重载
7. 标准函数驳回（private copy constructor）
8. public private
9. 效率，异常
10. 不够一般化，太过一般化
11. 是否真的需要这个类型

##条款20， 宁以pass by reference to const 替换 pass by value

- 区别

pass by value:

要调用copy constructor，可能是费时的操作

pass by reference to const:

const Student &s，const保证变量在函数内不会被修改

- pass by value可能导致多态失效

		void printNameAndDisplay（Window w）

传入子类对象，不能实现多态

- 在编译器底层，reference是通过指针来实现的

##条款21，必须返回对象时，别妄想返回其reference

	const Rational operator* (const Rational &lhs, const Rational &rhs)

如果返回reference

- 返回local stack的对象（Rational r），则函数退出时，这个对象已经被销毁了
- 返回heap-allocate对象，会造成何时delete的问题。
- 返回static对象，if(a*b == c*d),导致一个static对象不够用的问题

原则，必须在返回reference和object作出一个选择，程序员的工作就是选出正确的那个

##条款22，将变量声明为private

- public接口内全部都是函数，可以产生用户使用这个类时，良好的一致性
- private parameter可以产生封装的效果，封装使得变更更加容易
- 假如有一个public变量，如果取消它，所有使用它的客户代码都会被破坏
假如有一个protect变量，如果取笑它，所有使用它的derived class都会被破坏
所以protect并不比public更具有封装性


##条款23，宁以non-member、non-friend替换member函数

- 多个操作具有先后顺序，应该把他们绑定到一起
- 封装->客户端难修改->更多弹性去改变
- non-member（non-friend）函数VSmember函数

non-member函数不能访问private成分，提供更大的封装性

##条款24，若所有参数皆需类型转换，请为此采用non-member函数

实现有理数类Rational，乘法的操作符重载
开始可能会向使用成员函数的写法 

	const Rational operator*(const Rational & rhs) const

但是希望完成乘法交换律

	Rational r
	Rational result = 2 * r
需要对2进行隐式类型转换，方法

	const Rational operator*(const Rational &lhs, const Rational &rhs) 
使用non-member函数。

不是很明白

##条款25，考虑写出一个不抛出异常的swap函数

std::swap(T& a, T& b)可以对两个对象进行交换

如果这样做的效率不高，可以考虑自己写一个不会抛出异常的swap成员函数

例如：stl 容器中就有很多swap函数，只交换指针，而不会复制对象。

1. 自行实现这样一个swap成员函数(可以使用std::swap调换指针)
2. 在命名空间内提供一个swap<Widget>(Widget &a,Widget &b)去实现一个非成员函数来调用前者。


##条款26，尽可能延后变量定义式的出现时间

对变量进行定义，意味着承受构造的成本。

原则：应该延后变量定义到使用前的一刻为止。

##条款27，尽量少做转型动作

C风格的转型

	(int)2.1
	int(2.1)

C++的新式转型：

- `const_cast<T>(expression)` 将对象的常量性移除
- `dynamic_cast<T>(expression)` 主要用来进行安全向下转型
例如：只有基类可以使用，但是想调用子类的函数。尝试使用多态来代替。
- `static_cast<T>(expression)` 主要用来强制类型转换
例如：`static_cast<int>(2.1)`
尽量使用C++风格的转型

##条款28，避免返回handles指向对象内部成分

	class A{
	public：
	     void func();
	}
	class B{
	private:
	     A *a
	}

如果在B类中提供`A&`的返回（假设为rt），那么用户可以调用`rt.func()`修改B中的private成员了。
这是一种放松封装的行为。

##条款29，为“异常安全”而努力是值得的

异常安全的函数提供以下三个保证之一（从弱到强）：

- 基本承诺：如果抛出异常，程序内的任何事物仍然保持在有效状态下
- 强烈保证：函数调用成功，则完全成功。函数调用失败，则程序回复到调用之前的状态
- nothrow：保证绝对不抛出异常。（通常完全使用内置类型的操作，提供不抛出异常的保证）
一个软件系统，要么具备异常安全性，要么不具备。只提供部分异常安全性函数，不能叫做具备异常安全性的系统。
以对象管理资源，是一种很好的防止内存泄露，保证异常安全性的方法。

##条款30，透彻了解inlining的里里外外

- inline函数意味着对这个函数的每一次调用，使用函数本体替换

好处：减少调用成本

坏处：增加代码体积

- inline函数适合小型被频繁调用的函数

函数内部有for循环不适合inline，因为本身的开销已经够大，减少调用的开销意义不大。

- inline只是一个向编译器发出的申请，编译器可以忽略它。

如编译器拒绝复杂函数inline(带有递归，循环),virtual函数也会使inline落空。

##条款31，将文件间的编译依存关系降到最低

方法1，使用Handle class

增加一个实现类去真正实现类的功能，原来的类只维护一个指向实现类的指针

方法2，使用Interface class

基类是虚基类，不包括任何成员变量。

##条款32，确定你的public继承是is-a的关系

如题

##条款33，避免遮掩继承而来的名称

假如：Derived:Base

当编译器通过函数名称去找相应函数，会先从Derived类作用域找，然后再从Base类的作用域找
当使用函数重载的时候就可能出现问题。

使用using Base::func可以避免这种情况。 

##条款34，区分接口继承和实现继承

- 对于non-virtual函数的继承

意味着，子类必须有和父类一样的实现

- 对于virtual

（1）pure-virtual, 只继承接口，意味着每个子类的行为都很有可能不一样

（2）imprure-virtual， 提供缺省的实现，意味着有一些子类的行为可能一样

可以使用pure-virtual+缺省行为分离(另外写一个函数)的方法，解决有可能子类在不知情的情况下继承了并不需要的缺省的实现。 

## 条款35，考虑virtual函数以外的其他选择

- NVI Non-virtual Interface
使用public non-virtual 函数调用private virtual函数(做一下修饰而已)

- 使用函数指针

- 使用tr1::function封装函数指针，代替函数指针的行为

- 使用strategy设计模式

将想要virtual的行为封装成一个类(Calculator)，在类内部进行多态计算，通过传入的对象指针来判断。

##条款36，绝不重新定义继承而来的non-virtual函数

	class A{
	public:
	     void fun()
	}
	class B:A{
	public:
	     void fun()
	}
	A *ptA=new B()
	B *ptB=new B()
ptA->fun()调用A中的fun
ptB->fun()调用B中的fun
因为non-virtual函数不能进行动态绑定，调用函数只跟指针类型有关，所以

1. 不要重写父类的non-virtual函数
2. 父类的non-virtual函数意味着，所有子类的实现都是这样


##条款37，绝不重新定义进程而来的缺省参数值

缺省参数都是静态绑定的，即使是在virtual的函数中

##条款38，复合（组合）是has-a的关系
##条款39，明智而审慎地使用private继承

private继承意味着所有父类的成员在子类中都变为private，

好处：可以让基类部分最优化，减少尺寸。

##条款40，明智而审慎地使用多重继承

- 一个class继承自多个base class，那么父类成分有相同函数，就需要显示指定。
- 对于钻石型继承，B:A,C:A,D:B,D:C，需要指定虚继承，来避免重复继承A中的成分
- 虚继承需要编译器做很多工作，要付出一定成本，一般不用。
- 如果有单一继承可以满足需求，一般这个方案一定比多重继承要好。

##条款41，了解隐式接口和编译器多态

- 运行时多态，通过虚指针和虚函数实现

- 编译时多态

(1) 函数重载，相同函数名不同参数列表

(2) 在模板特化的时候，根据类型生成具体的函数


##条款42，了解typename的双重意义

	template< class T> class Widget;
	template<typename T>class Widget;
并没有什么不同

当使用嵌套从属名称，如：

	template<typename C>
	typename C::const_iterator iter(container.begin())
const_iterator是依赖于C的名称，这时候必须用typename

##条款43，学习处理模板化基类内的名称

对于模板C++的继承，由于基类模板可能被特化，特化使得基类内的成员不确定，C++会拒绝从模板化基类中寻找继承而来的名称

解决办法：

1. 在使用base class之前使用this->
2. 使用using

## 条款44，将与参数无关的代码抽离templates

使用带参template可能会引起代码膨胀，如：

	template<typename T,std:size_t n>

解决办法：
使用模板父类去处理由于size_t而造成的代码膨胀的问题

## 条款45，运用成员函数模板接受所有兼容类型的参数

- 智能指针是使用模板实现的，那如果我们要智能指针之间（具有继承关系的）能够相互转化，赋值，解决办法：
- 使用成员函数模板，对兼容的类型进行构造和赋值

##条款46，需要类型转换时请为模版定义非成员函数

	Rational<int> a(1,2);
	Rational<int> result = a*2; // Error

模板化实例，不进行隐式类型转换，使用friend方法。

##条款47，请使用traits classes表现类型信息

引用：
>traits class是个类模板，在不修改一个实体（通常是数据类型或常量）的前提下，把属性和方法关联到一个编译时的实体。在c++中的具体实现方式是：首先定义一个类模板，然后进行显式特化或进行相关类型的部分特化。
我的理解是：traits是服务于泛型编程的，其目的是让模板更加通用，同时把一些细节向普通的模板用户隐藏起来。当用不同的类型去实例化一个模板时，不可避免有些类型会存在一些与众不同的属性，若考虑这些特性的话，可能会导致形成的模板不够“泛型”或是过于繁琐，而traits的作用是把这些特殊属性隐藏起来，从而实现让模板更加通用。

## 条款48，认识template元编程

- 模版元编程有两个效力：第一，它让某些事情更容易；第二，可将工作从运行期转移到编译期。
- 引用：
>所谓元编程就是编写直接生成或操纵程序的程序，C++ 模板给 C++ 语言提供了元编程的能力，模板使 C++ 编程变得异常灵活，能实现很多高级动态语言才有的特性（语法上可能比较丑陋，一些历史原因见下文）。普通用户对 C++ 模板的使用可能不是很频繁，大致限于泛型编程，但一些系统级的代码，尤其是对通用性、性能要求极高的基础库（如 STL、Boost）几乎不可避免的都大量地使用 C++ 模板，一个稍有规模的大量使用模板的程序，不可避免的要涉及元编程（如类型计算）。

## 条款49，了解new_handler的行为

new_handler 的意思就是说，当使用operator new 无法分配内存时，转交给用户，用户来做一些事情。

## 条款50，了解new和delete的合理替换时机

有时候，我们替换掉编译器提供的new或者delete。重写operator new。三个常见理由：

1. 用来检测运用上的错误，超额分配一些内存，再额外的空间放置一些内存；
2. 为了强化效能，编译器提供的new/delete是通用的，通用就意味着冗余和效率低下，为什么？这个很好理解，因为他要支持很多情况下，也必须考虑很多情况。我们重写new/delete，也就是说，对于特定情况，给出特定的实现。
3. 为了收集使用上的统计数据。

## 条款51，编写new和delete时需固守常规

自定义new/delete的时候，需要遵守一些规则。

1. 循环申请，直到成功或者抛出异常
2. class专属版本处理，分配大小与class大小不一致的错误。
3. delete的时候，判断是否为null。

## 条款52，写了placement new也要写placement delete
## 条款53，不要轻忽编译器的警告
## 条款54，让自己熟悉包括TR1在内的标准程序库

C++11（原名C++0x）于2011年8月12日公布。
TR1是一份文档，由编译器实现，在std::tr1命名空间下
C++11纳入了大部分TR1的内容

## 条款55，让自己熟悉Boost

Boost是一个社区，提供很多程序库，作为新的C++标准的试验场。

