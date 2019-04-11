
### 一、MVVM双向数据绑定
>**Mvvm定义MVVM是Model-View-ViewModel的简写。即模型-视图-视图模型。**
>
>【模型】指的是后端传递的数据。<br>
>【视图】指的是所看到的⻚页⾯面。<br>
>【视图模型】mvvm模式的核⼼，它是连接view和model的桥梁梁。<br>
>它有两个⽅方向：
>1、是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的⻚面。实现的⽅方式是：数据绑定。<br>
2、是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的⽅方式是：DOM 事件监听。这两个⽅方向都实现的，我们称之为**数据的双向绑定**。

>**总结：**
>在MVVM的框架下视图和模型是不能直接通信的。
>它们通过ViewModel来通信，ViewModel通常要实现一个observer观察者，

>当数据发⽣生变化，ViewModel能够监听到数据的这种变化，
>然后通知到对应的视图做自动更新，
>而当用户操作视图，ViewModel也能监听到视图的变化，
>然后通知数据做改动，
>这实际上就实现了了数据的双向绑定。
>并且MVVM中的View 和 ViewModel可以互相通信

![](https://user-gold-cdn.xitu.io/2018/7/8/164783d90a67d0d9?w=873&h=462&f=jpeg&s=38226)
### 二、MVC
MVC是Model-View- Controller的简写。即模型-视图-控制器器。M和V指的意思和MVVM中
的M和V意思⼀一样。C即Controller指的是页⾯业务逻辑。使⽤用MVC的目的就是将M和V的代码分离。
**‘MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下**。

MVC和MVVM的**区别**并不是VM完全取代了了C，

ViewModel存在目的在于抽离Controller中展示的业务逻辑,
而不是替代`Controller`，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重⽤。<br>
由于mvc出现的时间比较早，前端并不那么成熟，很多业务逻辑也是在后端实现，<br>
所以前端并没有真正意义上的MVC模式。<br>
而我们今天再次提起MVC，是因为大前端的来到，出现了了MVVM模式的框架，我们需要了了解一下MVVM这种设计模式是如何一步步演变过来的。

### 三、为什么出现MVVM框架？

（我们用库，但是框架用我们，必须按它的规则做）

总结下来，一种两点，维护和使用方便、实现对业务的分成

在过去的10年年中，我们已经把很多传统的服务端代码放到了浏览器器中，这样就
产⽣生了成千上万行的javascript代码，它们连接了各式各样的HTML 和CSS⽂件，但缺乏正规的组织形式，这也就是为什么越来越多的开发者使用javascript框架。<br>
比如：`angular`、`react`、`vue`。浏览器的兼容性问题已经不再是前端的阻碍。

前端的项⽬越来越大，项⽬的可维护性和扩展性、安全性等成了主要问题。
当年年为了解决浏览器器兼容性问题，出现了很多类库，其中最典型的就是jquery。
但是这类库没有实现对业务逻辑的分成，所以维护性和扩展性极差。

综上两方⾯面原因，才有了了MVVM模式一类框架的出现。比如vue,通过数据的双向绑定，极⼤提高了开发效率。

### 四、Vue

**Vue就是基于MVVM模式实现的一套框架，**
在vue中：
Model：指的是js中的数据，如对象，数组等等。
View：指的是⻚页⾯面视图
viewModel：指的是vue实例例化对象

**为什么说vue是渐进式框架，什么是渐进式？**<br>

(1) 如果你已经有⼀一个现成的服务端应⽤用，你可以将vue 作为该应⽤用的⼀一部分嵌⼊入其中，带来更更加丰富的
交互体验;

(2) 如果你希望将更多业务逻辑放到前端来实现，那么VUE的核⼼库及其⽣态系统也可以满⾜足
你的各式需求（`core+vuex+vue-route`）。和其它前端框架一样，VUE允许你将一个⽹⻚分割成可复⽤的组件，每个组件都包含属于自⼰的HTML、CSS、JAVASCRIPT以⽤用来渲染网页中相应的地⽅。 <br>


(3) 如果我们构建⼀个大型的应⽤，在这一点上，我们可能需要将东⻄分割成为各自的组件和文件，vue有⼀个命令⾏工具，使快速初始化一个真实的工程变得非常简单（`vue init webpack my-project`）。我们可以
使用VUE的单文件组件，它包含了各自的HTML、`JAVASCRIPT`以及带作⽤用域的CSS或SCSS。 
以上这三个例子，是**一步步递进**的，也就是说对VUE的使用可大可小，它都会有相应的⽅方式来整合到你的项目中。所以说它是一个渐进式的框架。

**VUE最独特的特性**
>**响应式的（reactive）**，也就是说当我们的数据变更时，VUE会帮你更新所有网页中⽤到它的地⽅

### 五、主流框架实现双向绑定（响应式）的做法：
>1、 **脏值检查**：
>angular.js 是通过脏值检测的方式⽐比对数据是否有变更，来决定是否更新视图，最简单的方式就是通过 `setInterval()` 定时轮询检测数据变动，当然Google不会这么low.<br>
`angular`只有在指定的事件触发时进⼊入脏值检测，大致如下：
 DOM事件，譬如⽤用户输⼊入⽂文本，点击按钮等。
 ( `ng-click` ) XHR响应事件 ( `$http` ) 浏览器器Location变更更事件 (` $location` ) Timer事件(`$timeout` , `$interval` ) 执行 `$digest()` 或 `$apply()`在 `Angular` 中组件是以树的形式组织起来的。<br>
 >相应地，检测器器也是⼀一棵树的形状。当⼀一个异步事件发⽣生时，脏检查会从根组件开始，自上而下对树上的所有⼦组件进⾏行行检查，这种检查⽅方式的性能存在很⼤大问题。

**2、观察者-订阅者（数据劫持）：**

>`vueObserver` 数据监听器器，把⼀一个普通的 `JavaScript` 对象传给`Vue` 实例的 data 选项，
>`Vue` 将遍历此对象所有的属性，并使用`Object.defineProperty()`方法把这些属性全部转成`setter`、`getter`⽅方法。当`data`中的某个属性被访问时，则会调用getter方法，<br>
>当`data`中的属性被改变时，则会调用`setter`⽅方法。<br>
>`Compile`指令解析器器，它的作⽤对每个元素节点的指令进⾏解析，替换模板数据，并绑定对应的更新函数，初始化相应的订阅。<br>
>`Watcher` 订阅者，作为连接`Observer` 和` Compile` 的桥梁，能够订阅并收到每个属性变动的通知，执⾏指令绑定的相应回调函数。<br>
>`Dep` 消息订阅器器，内部维护了一个数组，用来收集订阅者（`Watcher`），数据变动触发`notify`函数，再调用订阅者的 `update` ⽅方法

``` javascript
Object.defineProperties(data,{
   name:{
     set(val){
       //console.log(this==data);true
       for(let item of inputs){
         if(item.getAttribute("v-model")=="name"){
           item.value=val;
         }
       }
       cloneList.forEach((item,index)=>{
         nodeList[index].innerHTML=item.innerHTML.replace(/\{\{name}}/g,()=>val)
       })
     },
     get(){
        // do something
     }
   },
   age:{}
 });
```