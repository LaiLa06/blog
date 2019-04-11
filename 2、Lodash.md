>
### 1、Lodash是什么？ 
Lodash是一个一致性、模块化、高性能的 JavaScript 实用工具库。它内部封装了诸多对字符串、数组、对象等常见数据类型的处理函数，

其中部分是目前ECMAScript尚未制订的规范，但同时被业界所认可的辅助函数。

### 2、为什么会出现Lodash？
任何一个库和框架的出现都是为了解决问题的，那么Lodash是解决什么问题的？

官方文档上的定义：
>Lodash 通过降低 array、number、objects、string 等等的使用难度从而让 JavaScript 变得更简单。
Lodash 的模块化方法 非常适用于：
>
>* 遍历 array、object 和 string
>* 对值进行操作和检测
>* 创建符合功能的函数

Lodash就是这样的一个工具库，方便我们在日常的开发中对数据的操作，特别是数组和对象的各种读写等操作，比如去重，拷贝，合并，过滤，求交集，求和等等，提高开发效率。
### 3、Lodash可用性怎么样？
每天使用npm安装Lodash的数量在百万级以上，这在一定程度上证明了其代码的健壮性。

在github上的数据也证明了这一点：

![](https://user-gold-cdn.xitu.io/2018/12/14/167ab68ff19d98b9?w=1462&h=142&f=png&s=62465)

Core build (~4 kB gzipped)
Full build (~24 kB gzipped)

完整版导出min版本的话大概78k，建议用的时候要根据场景按需引入 或者打包的时候不要打包进bundle里

### 4、Lodash怎么用？有什么特点？
#### (1) 安装
[Lodash地址](https://github.com/lodash/lodash) 
#### (2) 使用
[api文档](https://www.lodashjs.com/docs/4.17.5.html)
[使用范例](https://www.css88.com/archives/6275)
#### (3) 功能简介（版本4.17）
* Array，适用于数组类型，比如增删改查、填充数据(`_.fill`)、查找元素(`_.findeIndex`)、数组分片(`_.chunk`)等操作
* Collection，适用于数组和对象类型，部分适用于字符串，比如遍历、分组、查找、过滤等操作
* Function，适用于函数类型，比如节流、延迟、缓存、设置钩子等操作
* Lang，普遍适用于各种类型，常用于执行类型判断和类型转换、深拷贝
* Math，适用于数值类型，常用于执行数学运算(最大值、最小值求和)
* Number，适用于生成随机数，比较数值与数值区间的关系
* Object，适用于对象类型，常用于对象的创建、扩展、类型转换、检索、集合等操作
* Seq，常用于创建链式调用，提高执行性能（惰性计算）
* String，适用于字符串类型
* Util，一些工具函数，比如_.attmpt 替代 try-catch 的方式，当解析 JSON 出错时，该方法会返回一个Error对象
* Properties， 模板方法 ES6的模板字符串和这个功能很像（对模板字符串进行了正则解析，将需要填入数据的位置预留出来，拼接成一个字符串）
* Methods， 对lodash函数的引用
#### (4) 优势
##### 1、函数式编程风格
常用核心概念纯函数（注释1）、柯里化函数、声明式和命令式等
##### 2、链式调用和惰性求值

```javascript
//显式调用
var users = [
  { 'user': 'barney',  'age': 36 },
  { 'user': 'fred',    'age': 40 },
  { 'user': 'pebbles', 'age': 1 }
];
 
var youngest = _
  .chain(users)    //可以链式调用的关键 Returns the new lodash wrapper instance. 
  .sortBy('age')
  .map(function(o) {
    return o.user + ' is ' + o.age;
  })
  .head()
  .value();
// => 'pebbles is 1'
```
```javascript
//隐式调用
var youngest = _(users)    // 不需要显式的使用chain关键字
  .sortBy('age')
  .map(function(o) {
    return o.user + ' is ' + o.age;
  })
  .head()
```
注意两者的区别：

为什么要多出来一个 .value()，而不是直接出结果呢？那是因为可能要等待延时(Deferred execution)数据的到来，再执行取值。这就是我们常说的Lazy evaluation （延迟计算/惰性求值）

**_(value)建立了一个隐式链对象，可以把那些能操作并返回 arrays（数组）、collections（集合）、functions（函数）的”.Methods”（lodash的函数）串起来。 那些能返回“唯一值(single value)”或者可能返回原生数据类型（primitive value）会自动结束链式反应。 
而显式链则用_.chain. 的方式实现延迟计算，即：求值操作等到 _value()被调用时再执行。**

说到惰性求值官方这样介绍的：

>惰性求值（Lazy Evaluation），又译为惰性计算、懒惰求值，也称为传需求调用（call-by-need），是计算机编程中的一个概念，它的目的是要最小化计算机要做的工作。
惰性求值中的参数直到需要时才会进行计算。这种程序实际上是从末尾开始反向执行的。它会判断自己需要返回什么，并继续向后执行来确定要这样做需要哪些值。

以[How to Speed Up Lo-Dash ×100? Introducing Lazy Evaluation.](http://filimanjaro.com/blog/2014/introducing-lazy-evaluation/)中这个例子为例
```javascript
function priceLt(x) {
   return function(item) { return item.price > x; };
}
var gems = [
   { name: 'Sunstone', price: 4 }, { name: 'Amethyst', price: 15 },
   { name: 'Prehnite', price: 20}, { name: 'Sugilite', price: 7  },
   { name: 'Diopside', price: 3 }, { name: 'Feldspar', price: 13 },
   { name: 'Dioptase', price: 2 }, { name: 'Sapphire', price: 20 }
];
 
var chosen = _(gems).filter(priceLt(10)).take(3).value();
```
函数的目的是对数组gems进行筛选，选出3个price小于10的数据。

(1) 一般的做法（不用lodash）

`var chosen = _(gems).filter(priceLt(10)).take(3)`
先执行filter方法，遍历gems数组（长度为10），取出符合条件的数据：
```javascript
[
   { name: 'Sunstone', price: 4 },
   { name: 'Sugilite', price: 7  },
   { name: 'Diopside', price: 3 },
   { name: 'Dioptase', price: 2 }
]
```
然后，执行take方法，提取前3个数据。
总共遍历的次数为：10+3
执行的示例图如下：
![](https://user-gold-cdn.xitu.io/2018/12/14/167ab6ab429f096a?w=400&h=280&f=gif&s=702809)

(2) 惰性求值做法

以下是实现的思路：

1. _(gems)拿到数据集，缓存起来
2. 遇到filter方法，先记下来
3. 遇到take方法，先记下来
4. 遇到value方法，说明时机到了
5. 把小本本拿出来，看下要求：要取出3个数，price<10
6. 使用filter方法里的判断方法priceLt对数据进行逐个裁决

一共只执行了5次，就把结果拿到
![83f1785331176561cb98c745da91fe22.gif](https://user-gold-cdn.xitu.io/2018/12/14/167ab6b08706d384?w=400&h=280&f=gif&s=391224)
惰性计算的特点：<br>
1、**延迟计算**，把要做的计算先缓存，不执行 <br>
2、**数据管道**，逐个数据通过“裁决”方法，在这个类似安检的过程中，进行过关的操作，最后只留下符合要求的数据<br>
3、**触发时机**，方法缓存，那么就需要一个方法来触发执行。lodash就是使用value方法，通知真正开始计算<br>
### 5、为什么不用ES6完全替换Lodash？
目前替换不了，**就对null值容错方面上讲,Lodash有着更好的性能**
比如我们想从一组对象中摘取出某个属性的值
```javascript
let arr = [{ n: 1 }, { n: 2 }]
// ES6
arr.map(obj => obj.n)
// Lodash
_.map(arr, 'n')
```
当对象类型的嵌套层级很多时：
```javascript
let arr = [
 { a:[{ n:1 }]},
 { b:[{ n:1 }]}
]
// ES6
arr.map(obj => obj.a[0].n) 
// TypeError: 属性 'a' 在 arr[1] 中未定义
// Lodash
_.map(arr, 'a[0].n') 
// => [1, undefined]
```
许多 Lodash 提供的功能已经可以用 ES6 来替换，有些Lodash方法，ES6是没有的。Lodash 这样的类库也在不断推动 JavaScript 语言本身的发展


### 资料
[Lodash git地址](https://github.com/lodash/lodash)
[Lodash中文文档](https://www.lodashjs.com/)
[显试调用与隐性调用](https://blog.csdn.net/baidu_31333625/article/details/73188243)

注释1：纯函数：对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态的函数，比如redux中reducer。
