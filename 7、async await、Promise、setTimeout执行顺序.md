由题目见真知：

【题目】
```
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}
console.log('script start');
setTimeout(function(){
    console.log('setTimeout')
},0);
async1();
new Promise(function(resolve){
    console.log('promise1');
    resolve();
}).then(function(){
    console.log('promise2')
});
console.log('script end')
```
答案：
```
script start
async1 start
async2
promise1
script end
promise2
async1 end
setTimeout
```
相关知识点：

阮一峰老师的解释我觉得更容易理解：

> async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。
可以理解为，是让出了线程，跳出了async 函数体

然后,下面的这个输出顺序，和我们预想的不一样啊
```
async1 end
promise2
```
**==async==**

> async function 声明将定义一个返回 AsyncFunction 对象的异步函数。<br>
当调用一个 async 函数时，会返回一个 Promise 对象。<br>
当这个 async 函数返回一个值时，Promise 的 resolve 方法会负责传递这个值；<br>
当 async 函数抛出异常时，Promise 的 reject 方法也会传递这个异常值。

所以你现在知道咯，使用 async 定义的函数，当它被调用时，它返回的其实是一个Promise对象。

我们再来看看 await 表达式执行会返回什么值。

**await**
> 语法：[return_value] = await expression;
表达式（express）：一个 Promise 对象或者任何要等待的值。
> 返回值（return_value）：返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。

所以，当await操作符后面的表达式是一个Promise的时候，它的返回值，实际上就是Promise的回调函数resolve的参数。

明白了这两个事情后，我还要再啰嗦两句。**我们都知道Promise是一个立即执行函数，但是他的成功（或失败：reject）的回调函数resolve却是一个异步执行的回调。**<br>
**当执行到resolve()时，这个任务会被放入到回调队列中，等待调用栈有空闲时事件循环再来取走它。**

#### **解题思路：**

执行到 async1 这个函数时，首先会打印出“async1 start”（这个不用多说了吧，async 表达式定义的函数也是立即执行的）；

然后执行到 await async2()，发现 async2 也是个 async 定义的函数，所以直接执行了“console.log('async2')”，同时async2返回了一个Promise，划重点：**此时返回的Promise会被放入到回调队列中等待，await会让出线程（js是单线程还用我介绍吗），接下来就会跳出 async1函数 继续往下执行。**

然后执行到 new Promise，前面说过了promise是立即执行的，所以先打印出来“promise1”，然后执行到 resolve 的时候，resolve这个任务就被放到回调队列中（前面都讲过了上课要好好听啊喂）等待，然后跳出Promise继续往下执行，输出“script end”。

接下来是重头戏。同步的事件都循环执行完了，调用栈现在已经空出来了，那么事件循环就会去回调队列里面取任务继续放到调用栈里面了。

这时候取到的第一个任务，就是前面 async1 放进去的Promise，执行Promise时发现又遇到了他的真命天子resolve函数，划重点：**这个resolve又会被放入任务队列继续等待，然后再次跳出 async1函数 继续下一个任务。**

接下来取到的下一个任务，就是前面 new Promise 放进去的 resolve回调 啦 yohoo～这个resolve被放到调用栈执行，并输出“promise2”，然后继续取下一个任务。

后面的事情相信你已经猜到了，没错调用栈再次空出来了，事件循环就取到了下一个任务：历经千辛万苦终于轮到的那个Promise的resolve回调！！！执行它（啥也不会打印的，因为 async2 并没有return东西，所以这个resolve的参数是undefined），此时 await 定义的这个 Promise 已经执行完并且返回了结果，所以可以继续往下执行 async1函数 后面的任务了，那就是“console.log('async1 end')”。

然后同步(这里的回调是可以算成同步，有的时候勉强成异步)的执行完了，开始执行等待队列中的定时器，输出 setTimeout

---

#### 总结

1. 调用栈
2. 事件循环
3. 任务队列
4. promise的回调函数执行
5. async表达式的返回值
6. await表达式的作用和返回值

**队列任务优先级：`promise.Trick()`>`promise的回调`>`setTimeout`>`setImmediate`**

至此，该题的输出结果分析完毕了，这类的执行结果可以用一句话总结，先执行同步代码，遇到异步代码就先加入队列，然后按入队的顺序执行异步代码，最后执行setTimeout队列的代码。

原文地址：https://lvdingjin.github.io/tech/2018/05/27/async-and-await.html
