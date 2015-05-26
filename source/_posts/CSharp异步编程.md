title: "C# 异步编程"
date: 2014-03-07 19:41
category: [.NET]
tags: [C#]
---

##为什么要使用异步编程
普通程序单线程运行，如果遇到I/O操作，大量数据库操作，使用普通方法会使得当前线程阻塞。使用异步编程就会另外开启一个线程并行 ，解决了这些问题。

##常见异步过程
第一步：准备一个准备进行异步调用的方法，如TestMethod

```
class AsyncDemo
{
    public string TestMethod(int callDuration,out int threadId)
    {
        Console.WriteLine("Test method begins.");
        Thread.Sleep(callDuration);
        threadId = AppDomain.GetCurrentThreadId();
        return "MyCallTime was " + callDuration.ToString();  
    
}
```

第二步：和这个方法对应的委托

```
public delegate string AsynDelegate(int callDuration,out int threadId);
```
第三步：声明委托变量，并将第一步中的方法绑定到这个变量上去

```
int threadId;
AsyncDemo ad = new AsyncDemo();
AsynDelegate asynDelegate = new AsynDelegate(ad.TestMethod);
```
第四步：进行异步调用

```
IAsyncResult ir = asynDelegate.BeginInvoke(3000, out threadId, null, null);
```
第五步：调用结束后，收回线程资源

```
string ret = asynDelegate.EndInvoke(out threadId, ir);
```
##线程回收方式
在C#中，用户启动的线程资源是必须收回的，因为浪费可耻。启动线程容易，但是何时收回，如何收回就比较值得考虑了，根据收回的方式，可以将异步调用，分为两种方式，一种是 **主动收回** （通过主线程阻塞，等待异步线程完成，然后在主线程收回线程资源），**被动收回**（当异步线程运行完后，触发回调函数，在回调函数中收回线程资源）。
###阻塞主线程
有三种方式
1. 使用EndInvoke直到结束
2. 使用waithandle等待调用结束
3. 轮询异步调用完成 

####使用EndInvoke直到结束
 

```
IAsyncResult ir = asynDelegate.BeginInvoke(3000, out threadId, null, null);
string ret = asynDelegate.EndInvoke(out threadId, ir);//通过out可以将异步调用中的变量传递出来，为什么需要ir？程序需要通过ir获得异步委托，从而进行线程资源的收回
```

BeginInvoke前两个参数是声明TestMethod函数的时候的参数，第三个是**回调函数委托** ，第四个，当使用回调函数时需要将asynDelegate作为参数传入，从而使得在异步结果（ir）中能够拿到asynDelegate变量，从而进行相关操作。
EndInvoke 前面是testMethod中ref和out参数，后面是异步状态变量IAsyncResult

####使用waithandle等待调用结束

```
IAsyncResult ar = asynDelegate.BeginInvoke(3000, out threadId, null, null);    
// C#异步调用完成时会发出 WaitHandle 信号 ，程序在此阻塞
ar.AsyncWaitHandle.WaitOne();
string ret = asynDelegate.EndInvoke(out threadId, ar);  
```
####轮询异步调用完成

```
IAsyncResult ar = asynDelegate.BeginInvoke(3000,out threadId, null, null);
//轮询ar.IsCompleted的状态
while(ar.IsCompleted == false) {  
    Thread.Sleep(10);  
}  
//会导致程序在此阻塞  
string ret = asynDelegate.EndInvoke(out threadId, ar);  
```
###使用回调函数收回线程资源
####声明回调函数

```
static void CallbackMethod(IAsyncResult ar) {
    int threadId;
    // 通过异步结果（IAsyncResult）重新得到委托对象
    AsynDelegate asynDelegate = (AsynDelegate)ar.AsyncState;
    //在回调函数中调用EndInvoke，收回异步线程的资源
    string ret = asynDelegate.EndInvoke(out threadId, ar);  
    Console.WriteLine("The call executed on thread {0}, with return value \"{1}\".", threadId, ret);  
    }
```
####将回调函数作为异步调用的参数，进行异步调用

```
IAsyncResult ar = asynDelegate.BeginInvoke(3000, out threadId, new AsyncCallback(CallbackMethod), asynDelegate);
```
##异步操作与多线程
当需要执行I/O操作时，使用异步操作比使用线程+同步 I/O操作更合适。I/O操作不仅包括了直接的文件、网络的读写，还包括数据库操作、Web Service、HttpRequest以及.net Remoting等跨进程的调用。

而线程的适用范围则是那种需要长时间CPU运算的场合，例如耗时较长的图形处理和算法执行。但是往 往由于使用线程编程的简单和符合习惯，所以很多朋友往往会使用线程来执行耗时较长的I/O操作。这样在只有少数几个并发操作的时候还无伤大雅，如果需要处 理大量的并发操作时就不合适了。

