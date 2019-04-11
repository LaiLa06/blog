node.js中的非IO的异步API提供了四种方法，分别为`setTimeOut()`,`setInterval()`,`setImmediate()`以及`process.nextTick()`，四种方法实现原理相似，但达到的效果略有区别：
#### 一、事件循环Event Loop
首先，我们需要了解node.js的基于事件循环的事件模型，正是因为它才使得node.js中回调函数十分普遍，也正是基于此，node.js实现了单线程高效的异步IO（这里说的单线程主要说的是执行javascript代码部分的线程，而异步IO部分node.js其实还是利用线程池去执行的）。

简单的讲就是，在node.js启动时，创建了一个类似while(true)的循环体，每次执行一次循环体称为一次tick，每个tick的过程就是查看是否有事件等待处理，如果有，则取出事件极其相关的回调函数并执行，然后执行下一次tick。所以，有如下代码：
```
A();
B();
C();
```
它的执行逻辑是，先询问事件观察者当前是否有任务需要执行？观察者回答“有”，于是取出A执行，A是否有回调函数？如果有（如果没有则继续询问当前是否有任务需要执行），则取出回调函数并执行(注意：回调函数的执行基本都是异步的，可能不止一个回调)，执行完回调后通过某种方式通知调用者，我执行完了，并把执行结果给你，你自己酌情处理吧，主函数不需要不断询问回调函数执行结果，回调函数会以通知的方式告知调用者我执行完了（don’t call me ,i will call you.），而这个过程主线程并不需要等待回调函数执行完成，它会继续向前执行，即再次询问观察者当前是否还有任务需要执行，重复上面的步骤。。。直到观察者回答没有了，线程结束。

整个事件循环的逻辑如下图：
![](https://user-gold-cdn.xitu.io/2018/6/6/163d35d2a274f76b?w=408&h=432&f=png&s=21758)
#### 二、setTimeOut(),setInterval(),setImmediate()以及process.nextTik()
这里面setTimeOut()与setInterval()除了执行频次外基本相同，都表示主线程执行完一定时间后立即执行，而setImmediate()与之十分相似，也表示主线程执行完成后立即执行。那么他们之间的区别是什么呢？

代码如下：
```
setTimeout(function(){
    console.log("setTimeout");
},0);

setImmediate(function(){
    console.log("setImmediate");
});
```
两者都代表主线程完成后立即执行，其执行结果是不确定的，可能是`setTimeout`回调函数执行结果在前，也可能是`setImmediate`回调函数执行结果在前，但`setTimeout`回调函数执行结果在前的概率更大些，这是因为他们采用的观察者不同，`setTimeout`采用的是类似IO观察者，`setImmediate`采用的是check观察者，而`process.nextTick()`采用的是**idle观察者**。

**三种观察者的优先级顺序是：idle观察者>>io观察者>check观察者**

process.nextTick()与setImmediate()和setTimeout()的区别如下：

1、原始代码：
```
A();
B();
C();
```
它的执行顺序即代码顺序：

![](https://user-gold-cdn.xitu.io/2018/6/6/163d363b3c25e9b7?w=931&h=316&f=png&s=19288)
2、process.nextTick()执行效果，代码如下：
```
A();
process.nextTick(B);
C();
```
它的执行顺序如下：
![](https://user-gold-cdn.xitu.io/2018/6/6/163d364e49077f70?w=915&h=297&f=png&s=20602)
3、setImmediate()或者setTimeout()执行效果，代码如下：
```
A();
setImmediate(B);//或者setTimeout(B,0);
C();
```
它的执行顺序如下：

![](https://user-gold-cdn.xitu.io/2018/6/6/163d365b1a3a3d3f?w=832&h=332&f=png&s=21423)

结论：<br>
`process.nextTick()`，效率最高，消费资源小，但会阻塞CPU的后续调用； 
`setTimeout()`，精确度不高，可能有延迟执行的情况发生，且因为动用了红黑树，所以消耗资源大； <br>
`setImmediate()`，消耗的资源小，也不会造成阻塞，但效率也是最低的。

转载https://blog.csdn.net/hkh_1012/article/details/53453138