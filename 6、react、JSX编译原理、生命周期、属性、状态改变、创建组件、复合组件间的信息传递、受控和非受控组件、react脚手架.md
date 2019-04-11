
> React是Facebook公司研发的一款JS框架（MVC：Model View Controller）
> **通过数据的改变来影响视图**
> 
### 1、`React`脚手架
React是一款框架：具备自己开发的独立思想    - 划分组件开发

**前端工程化开发：**
- 基于框架的组件/模块化开发
- 基于webpack的自动部署

 >webpack来完成以上内容（自动化）：
>- 基于路由的spa单页面开发
- 区分开发环境和生产环境
- 安装babel完成ES6编写代码（上线时，ES6编译成ES5）
- 安装css、style、file等处理器，处理合并压缩的
- 编译less，sass等预编译语言
- 安装webpack-dev-server ，可以在开发环境下，编译后自动创建服务，打开浏览器，当代码修改后，自动保存编译，页面自动刷新
- ....

**脚手架**
配置webpack相对复杂，我们需要安装很多的包，还需要写很多相对复杂的配置，这时候脚手架应运而生，用来提升我们开发的效率

vue：vue-cli
react：create-react-app（应用）

### 二、安装
1、`npm install create-react-app -g `   // 安装到全局能用命令
（` npm root -g`  查看全局npm安装目录）
2、`create-react-app demo1 `        // 项目名称demo1，名称只能（/^[a-z0-9_-]$/）

**脚手架生成目录中的一些内容：**
>1、node_module 当前项目中依赖的包
   .bin 本地项目中可执行的命令，在package.json的scripts中配置的对应的脚本即可（还有一个是react-scripts命令）
2、public 存放当前项目的页面（单页面应用放index.html即可，多页面根据自己需求放置需要的页面）

在react中，**所有的逻辑都是在JS中完成的**（包括页面机构的创建），如果想给当前的页面导入一些css样式后者img图片等内容，我们有两种方式：
>(1)、在JS中基于ES6 Module模块规范，使用import引入，这样webpack在编译合并JS的时候，会把导入的资源文件等插入到页面的机构中（绝对不能在js管控的结构中通过相对目录./ 或者../  导入资源，因为在webpack编译的时候，地址就不在是之前的相对地址**要用绝对地址**）
>
(2)、如果不想在js中导入（JS中导入的资源最后都是基于webpack编译），我们也可以把资源手动的在html中代入，但是html最后也要基于webpack编译，导入的地址也不建议写相对地址，而是使用`%PUBLIC_URL%`写成绝对地址
```
<link rel="manifest" href="%PUBLIC_URL%/reset.min.css">
```
3、src  项目结构中最主要的目录，因为后期所有的JS、路由、组件等都是放到这里面（包括需要编写的css或者图片等）
index.js 是入口文件
4、.gitignore   git的忽略文件
5、package.json 当前项目的配置清单
```
  "dependencies": {
    "react": "^16.4.1",
    "react-dom": "^16.4.1",
    "react-scripts": "1.1.4"
  },
```
基于脚手架生成工程目录，自动帮我们安装了三个模块：
- react 、
- react-dom、
- react-scripts（集成了webpack需要的内容babel，css处理，eslint, webpack....，注意：没有less、sass）

package.json：
```
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
```
可执行的脚本

`npm run start`    或者  `yarn start`
>`yarn start`：
>- 创建一个默认端口号为3000 ，协议为http的web服务
>- 按照webpack.config.dev.js把项目编译
>- 打开浏览器预览我们的项目
>- 项目修改时候，自动重新编译，浏览器页面自动刷新，展示最新的效果
>
>`yarn build:`

>- 基于webpack.config.prod.js  把项目进行编译打包
>- 生成一个build文件夹，存放最后打包的文件>- 生成一个build文件夹，存放最后打包的文件
>- 部署上线的时候，只需要把build的内容发布即可
>

### 三、React脚手架的深入剖析

create-react-app 脚手架为了让结构目录清晰，把安装的webpack及配置文件都集成在了react-scripts模块中，放到了node_modules中
但是真实项目中，我们需要在脚手架默认安装的基础上，额外安装一些我们需要的模块，例如`react-router-dom `、`axios`、还有`less`、`less-loader`

>**情况一**：如果我们安装其他的组件，但是安装成功后不需要修改webpack的配置项，此时我们直接安装，调取使用就行，比如react-router-dom 、axios等

>**情况二**：我们安装的插件是基于webpack处理的，也就是需要把安装的模块配置到webpack中（重新修改webpack配置项了）
>1、首先需要把隐藏到node_module中的配置项暴露到项目中
`yarn eject`
首先会确定是否确认执行eject,这个操作不可逆转，一旦暴露出来配置项，就无法再隐藏了
如果还有未提交到历史区的内容，需要先提交，然后才能eject
>2、再去修改对应的配置项
暴露后，项目目录中多了两个文件夹：
config：存放的是webpack的配置文件
（1）`webpack.config.dev.js ` 开发环境下的配置项（yarn start）
（2）`webpack.config.prod.js`  生产环境下的配置项（yarn build）
scripts：存放的是可执行脚本的JS文件
（1）start.js: `yarn start` 执行的就是这个JS
（2）build.js:  `yarn build` 执行的就是这.个JS

>我们预览项目的时候，也是基于webpack编译，把编译后的内容放到浏览器中运行，所以如果项目中使用了less，我们需要修改webpack配置项，在配置项中加入less的编译工作，这样后期预览项目，首先基于webpack把less编译为css

`set HTTPS=true && npm start`  开启HTTPS协议模式
`set PORT=1111 && npm start` 修改默认端口号

### 四、渐进式框架
> 一种渐进式框架设计思想，一般框架中都包含了很多内容，这样导致框架的体积过于臃肿
> 拖慢加载的速度，
> **真实项目中，我们使用一个框架，不一定用到所有的功能，此时我们应该把框架的功能进行拆分，用户想用什么，让其自己自由组合即可**--"**渐进式框架**"

**全家桶**：渐进式框架N多部分的组合
**vue全家桶**：vue、vue-cli、vue-router、vuex、axios、vue-element 、vant、
**react全家桶**：react、react-react-app、react-dom、react-router、redux、react-redux、axios、ant、dva、saga、mobx

#### 1、React
>react的核心部分，提供了Component类可以进行组件开发，提供了钩子函数（生命周期），所有的生命周期函数都是基于回调函数完成的
#### 2、React-dom：
>raect独有的，把JSX语法，渲染为真实DOM（能够放到页面中展示的机构都叫做真实的DOM）的组件
>
**react框架都是在JS中进行的，然后通过webpack编译，再放到浏览器中编译**

在index.js这个入口文件：
```
import React from 'react';
import ReactDOM from 'react-dom';
// import ReactDOM,{render} from 'react-dom';
// 从react-dom中导入一个reactDOM，逗号后面的内容是把ReactDOM这个对象进行结构

let data ='zufeng',
    root = document.querySelector("#root");

ReactDOM.render(<div id="box">hello world!{data}</div>,root,()=>{
    // 回调,一般不用
    let oBox = document.querySelector("#box");
    console.log(oBox.innerHTML);
});
```

#### 2.1、JSX（也叫react元素）：
**JSX语法的渲染使用的是ReactDOM.render:**
`ReactDOM.render([jsx],[container],[callback]);`

>**jsx**：react独有的语法， JavaScript+xml（HTML），和我们之前拼接的字符串类似，都是把html结构代码和js代码或者数据混合在一起了，但是他不是字符串
**container**：容器，我们想把元素放到页面中的哪个容器中
**callback**：当把容器放到页面中呈现触发的回调函数

**jsx语法特点：**
>1、不建议我们 直接把jsx直接放到body中，而是放在自己创建一个容器中，一般我们都放在一个id为root的div中
```
ReactDOM.render(<section><h2>内容</h2></section>,root)
```
2、把数据嵌入到jsx中
在JSX中出现的`{}`是存放JS的，要求JS代码执行完成需要**有返回结果**（JS表达式），不能直接放一个对象数据类型的
```
let name = 'zhufeng'     // 基本数据类型的值
ReactDOM.render(<section><h2{name}</h2></section>,root)

let xx = {name:'xxxx'}
ReactDOM.render(<section><h2{xx}</h2></section>,root)  // 报错
```
- 不能直接放一个对象数据类型的值（对象（(除了给style赋值)），数组（如果只有基本值或者jsx除外），函数都不行）
```
ReactDOM.render(<ul>
   {
     data.map((item,index)=>{
        return <li key={index}>{item.title}</li>
     })
   }
</ul>,root)
```
- 可以是基本类型的值（布尔类型什么都不显示，null，undefined也是jsx元素，代表的是空）
- 循环判断语句都不行，但是支持三元运算符

>3、循环数组穿件jsx元素，需要给创建的元素设置唯一的key值（当前本次唯一即可）
4、只能出现一个根元素
5、给元素设置样式类用的是className而不是class
6、style中不能直接写样式字符串，需要基于一个样式对象来遍历赋值
7、可以给JSX元素设置属性：
=> 属性值对应大括号中，对象、函数都可以放（也可以放JS表达式）
=>style属性值必须是对象（不能是字符串）`<li style={{color:'#fff'}}></li>`
=>class 用className代替

```
ReactDOM.render(<h1 id={'box'} className="box" style={{color:'red'}}>我是标题</h1>);
```
#### 3、渲染原理（react的核心原理之一）


**JSX虚拟DOM变为真实的DOM**（react的核心原理之一）
```
let  styleObj = {color:'red'};
ReactDOM.render(<h1 id="titleBox" className='title' style={styleObj.color}></h1>)
```
**JSX渲染机制：JSX->真实DOM**
>1、基于babel babel-loader babel-present-react-app把JSX语法编译为`React.createElement([type],[props],[children])`结构
>=>React.createElement中至少有两参数，
>type：第一参数的标签（字符串）
>props：属性对象（没有就是null）
>其他的：都是子元素内容（只要子元素是html，就会变成新的createElement）
```
var styleObj = { color: 'red' };
ReactDOM.render(React.createElement('h1', { id: 'titleBox', className: 'title', style: styleObj.color }));
```
>2、执行`React.createElement(....)`，创建一个对象（虚拟DOM）

![](https://user-gold-cdn.xitu.io/2018/6/29/164495d0633906a4?w=755&h=250&f=jpeg&s=14048)
>=>key 和ref 都是createElement中的Prop提取出来的
=>props:{
   chiledren：存放自己子元素的（没有子元素就没有这个属性），如果有多个子元素，就以数组的形式存储信息
}
3、`ReactDOM.render(JSX语法最后生成的对象,container,callback)`，基于render方法把生成的对象动态创建为DOM元素，插入到指定的容器中

#### 4、组件
> 不管vue还是react框架，设计之初都是期望我们按照“组件 / 模块管理” 的方式来构建程序的，也就是一个程序划分一个个的组件来单独管理
`src -> component`：这个文件夹下存放的就是开发用的组件
> 【优势】
> 1、有助于多人协助开发
> 2、组件可以复用
> ....

**react 中创建组件有两种方式：**

1、函数式声明组件
>（1）函数返回结果是一个新的JSX（也就是当前组件的JSX结构）
（2）props变量存储的值是一个对象，包含了调取组件时候传递的属性值,不传递的话是个空对象

**知识点**：
>createElement遇到一个组件，返回的对象中：type就不是字符串标签名了，而是一个函数（类），但是属性还是存在props中

**render渲染的时候，我们需要做处理：**
>首先判断type的类型，
>如果是字符串，就创建一个元素标签，
>如果函数或者类，就把函数执行，把props中的每一项（包含children）传递给函数
在执行的时候，把函数中return的JSX转换为新的对象（通过createElement），把这个对象返回：紧接着`render`按照以往的渲染方式，创建DOM元素，插入到指定的容器中即可

>单闭合和双闭合组件的区别，双闭合组件中可以放子孙元素
>
>**函数式声明特点：**
>
>1、会把基于createElement解析出来的对象中的props作为参数传递给组件（可以完成多次调取组件传递不同的信息）
>2、一旦组件调取成功，返回的`jsx`就会渲染到页面中，但是后期不通过重新调取组件或者获取DOM元素操作的方式，很难再把渲染好组件中的内容更改   -->"**静态组件**"
```
//Dialog.js
import React from 'react';    // 每个组件必须引入，因为需要基于它的create-element把JSX进行解析渲染
export default function Dialog(props) {
    //props:调取组件的时候传递进来的属性信息（可能包含className、style、id、可能有children）
    let {con, lx = 0, children,style={}} = props,
        title = lx === 0 ? '系统提示' : '系统警告';
    //children 可能有，可能没有，可能是个值，也可能是个数组，每一项可能是字符串也可能是个对象等，都代表双闭合组件的子孙元素
    return <section style={style}>
        <h2>{title}</h2>
        <div>{con}</div>
        {/*1、把属性中的子元素，放到组件中的指定位置*/}
        {/*{children}*/}
        {/*2、也可以使用react中专门遍历children的方法*/}
            {children.map(item => item)}
            {/*// {React.Children.map(children,item=>item)}*/}
        </section>
}

// index.js

import React from 'react';
import ReactDOM from 'react-dom';
// 公共css放index.js中，这样在其他组件中也可以使用（webpack会把所有的组件最后都编译到一起，index是主入口）
// import 'static/css/reset.min.css'
import 'bootstrap/dist/css/bootstrap.css'  // 需要导入未经过压缩的文件，否则报错（真实项目中bootstrap已经是过去式了，后期用ant）

import Dialog from "./component/Dialog-bootstrap";

ReactDOM.render(<main>
    <Dialog content ='马少帅很帅'/>
    <Dialog type ={2} content='系统错误了'/>
    <Dialog type = '请登录' content={
        '新的JSX语法'
    }>
        <div>
            <input type="text" className='form-control' placeholder='请输入用户名'/><br/>
            <input type="password" placeholder='请输入密码'/>
        </div>
        <br/>
        <button className='btn btn-success'>登录</button>
        <button className='btn btn-danger'>取消</button>
    </Dialog>
</main>,root);
```
2、继承component类来创建组件
>基于component把JSX转换为一个对象，当render渲染这个对象的时候，遇到type是一个函数或者类，不是直接创建元素，而是把方法执行，
- 如果就是函数声明的组件，就把它当做普通函数执行（方法中的this是undefined），把函数返回的JSX元素（也是解析后的对象）进行渲染
- 如果是类声明式的组件，会把当前类new执行，创建类的一个实例（当前本次调用的组件就是它的实例）执行constuctor之后，会执行this.render()，把rende返回的JSX拿过来渲染，所以“**类声明式的组件，必须有一个render的方法，方法中需要返回一个JSX元素**”
但是不管哪一种方式，最后都会把解析出来的props属性对象作为实参传递给对应的函数或者类

**特点：**
1、调取组件相当于创建类的实例（this），把一些私有的属性挂载到实例上了，这样组件内容所有的方法中都可以基于实例获取这些值（包括：传递的属性和自己管理的状态）
2、有自己的状态管理，当状态改变的时候，react会重新渲染视图（差异更新：基于DOM-DIFF只把有差异的部分渲染）
```
import React from 'react';
import ReactDOM from 'react-dom';
import PropTypes from 'prop-types'  // facebook是公司开发的一个插件，基于这个插件我们可以给组件传的属性设置规则，设置的规则不会影响页面的渲染，可以不加
// 但是会在控制台抛出警告错误

class Dialog extends React.Component{
   // this.props只读的，我们无法在方法中修改它的值，但是可以给其设置默认值或者设置一些规则（例如：设置是否是必须传递的以及传递的值的类型等）
   static  defaultProps ={
        lx:"系统提示"
   };
   static propTypes = {
        // 设置属性规则，如果传的不是这个规则的，不影响页面的渲染，只是会在控制台抛出警告错误
        // con: PropTypes.string   传递的内容必须是字符串
        con:PropTypes.string.isRequired  // 传递的内容是字符串，而且还必须是传递
   };
   constructor(props){    // props context updater
       // props：当render渲染并且把当前类执行创建实例的时候，会把之前JSX解析出来的props对象中的信息（可能有children）传递给参数props  =>  "调取组件传递的属性"

       // props如果不传，super也不传，除了constructor中不能直接使用this.props,其他声明周期函数中都可以使用（也就是执行完成constructor，
       // react已经帮我们把传递的属性接收，并且挂载到实例上了）

       super(props);  // extends继承，一旦使用了constructor，第一行位置必须设置super执行，相当于React.Component.call(this),也就是call继承，把父类私有的属性继承过来
       // this.props:属性集合
       // this.refs：ref集合（非受控组件中用到）
       // this.context:上下文

       // - 如果super(props); 在继承父类私有的时候,就把props挂载到this实例上了，这个this只是constructor中的this，不影响原型上的this，写不写都行
       // - 如果只写super() 虽然创建实例的时候把属性传递过来了，但是并没有传递父组件，也就是没有把属性挂载到实例上，使用this.props获取的结果是undefined
       // -
       console.log(props);
       // AA = 12;  设置私有属性，但是不符合ES6语法，需要babel-preset-react编译
       // fn=()=>{}  设置一个箭头函数，但是不符合ES6语法，需要babel-preset-react编译
   }

   render(){
       // render必须写还必须有返回的东西
       return <section>
           <h3>系统提示</h3>
           <div>
               zhufeng
           </div>
       </section>
   }
}

ReactDOM.render(<div>
    <Dialog>
        <span>d</span>
    </Dialog>
</div>,root);
```
组件中的属性（this.props）是调取组件的时候（创建类实例的时候）传递给组件的信息，这部分信息是只读的（只能获取不能修改）-> "组件的属性是只读的"
```
Object.defineProperty(this.props,'cont',{
   writable:true
});
```
用这种方法也改不了
       
----------

**组件部分的总结**

创建组件有两种方式：一是“函数式”，一是“创建类式”

【**函数式**】
>1、操作简单
2、能实现的功能也很简单，只是简单的调取和返回

【**创建类式**】
>1、操作相对复杂一点，但是也可以实现更为复杂的业务功能
2、能够使用生命周期函数操作业务
3、函数式可以理解为静态组件（组件中内容调取的时候，就已经固定了，很难再修改），而类这种方式，可以基于组件内部的状态来动态更新渲染的内容: 

所谓**函数组件是静态组件**，和执行普通方法一样，调取一次组件，就把组件中内容获取到，如果不重新调取组件，显示内容是不发生改变的

----------

**【需求】实现页面上时间的走动**

**1、函数式的**
```
import React from 'react';
import ReactDOM from 'react-dom';

function Clock() {
  return <div>
      <h3>当前北京时间为：</h3>
      <div style={{color:'red',fontWeight:'bold'}}>{new Date().toLocaleString()}</div>
  </div>
}
/*每隔一秒重新调取组件*/
setInterval(()=>{
    ReactDOM.render(<Clock/>,root);
},1000)
//适用于组件内容不会再次发生改变的情况下
```
**2、创建类式**
```javascript
class Clock extends React.Component{
   constructor(){
       super();
       // 初始化组件状态(都是对象类型):要求我们在constructor中把后期需要使用的状态信息全部初始一下（约定俗称的语法规范）
       this.state = {
           time:new Date().toLocaleString(),
       }
   }
   componentDidMount(){
       //react生命周期函数之一：第一次组件渲染完成后触发（我们只需要间隔1000ms把state状态中的time数据改变，这样react会帮我们组件中的部门内容进行渲染）

       setInterval(()=>{
           // this.state.time = new Date().toLocaleString();  //虽然下面的定时器可以修改状态，但是不会通知react重新渲染页面，这样不行
           //修改组件的状态
            //  1、修改部分状态：会用我们传递的对象和初始化的state进行匹配，只把我们传递的属性进行修改，没有传递的依然保留原始的状态信息（部分修改）
            //  2、修改状态修改完成，会通知react把组件进行重新渲染
           
           this.setState({
               time:new Date().toLocaleString(),
           },()=>{
               /*当通知react把需要重新渲染的JSX元素重新渲染完成后，执行的回调操作（类似于生命周期中的componentDidUpdate）项目中一般使用钩子函数*/
               // 设置回调的原因是：通知完就直接往下执行，render方法是个异步操作
           });
       },1000)
   }
   render(){
       return <div>
           <h3>当前北京时间为：</h3>
           <div style={{color:'red',fontWeight:'bold'}}>{this.state.time}</div>
       </div>
   }
}
ReactDOM.render(<Clock/>,root);
```
**React中的组件有两个非常重要的概念：**
>1、组件的属性：[只读]调取组件的时候传递进来的信息
2、组件的状态：[读写]自己在组件中设定和规划的（只有类声明式组件才有状态的管控你，函数式组件声明不存在状态的管理）

组件状态类似于VUE中的数据驱动：
>我们数据绑定的时候是基于状态值绑定，当修改组件状态后，对应的JSX元素也会跟着重新渲染（差异渲染：只把数据改变的部分重新渲染，基于`DOM-DIff`算法完成）

当代前端框架最重要的核心思想就是：“**数据操作视图（视图影响数据）**”，让我们告别JQ手动操作DOM的时代，我们以后只需要改变数据，框架会帮我们重新渲染视图，从而减少直接操作DOM（提高性能，也有助于开发效率）

**尽量少操作DOM**

### 五、React的用法

在react当中：
1、基于数据驱动（修改状态数据，react帮助我们重新渲染视图）完成的组件叫做“受控组件（受数据管控的组件）”
2、基于ref操作DOM实现视图更新，叫做“非受控组件”
真实项目中，建议多使用“受控组件”

VUE：MVVM 数据更改，视图跟着改变，视图更改，数据也跟着改变（双向数据绑定）
React：MVC 数据更改视图跟着改变（原本是单向的，但是我们可以手动设置为双向的）
```
render(){
  let {text} = this.state;

  return <section className='panel panel-default'>
      <div className='panel-heading'>
          <input type="text" className='form-control' value={text} onChange={ev=>{
              // 在onChange中修改状态信息，实现的是视图改变数据
              this.setState({
                  text:ev.target.value
              })
          }}/>
      </div>
      <div className='panel-body'>
          {text}
      </div>
  </section>
}
```
### 六、生命周期
>所谓生命周期函数（钩子函数）描述一个函数或者组件从创建到销毁的过程，我们可以在过程中间，基于钩子函数完成自己的一些操作（例如：在第一次渲染完成做什么，或者在第二次即将重新渲染之前做什么等...）

#### 1、基本流程
>【基本流程】
>`constructor`： 创建一个组件
>`componentWillMount`： 第一次渲染之前
`render`：第一次渲染
`componentDidMount`： 第一次渲染之后
【修改流程】
当组件的状态数据发生改变（setState）或者传递的属性发生改变（重新调用组件，传递不同的属性）都会引发render重新执行渲染（渲染也是差异渲染）
`shouldComponentUpdate`  是否允许组件重新渲染
`componentWillUpdate`   重新渲染之前
`render`  第二次及以后重新渲染
`componentDidUpdate` 重新渲染之后

>`componentWillReceiveProps` 父组件把传递给子组件的属性发生改变后触发的钩子函数
>属性改变也会改变子组件重新渲染，触发钩子函数

>【销毁】
>原有的渲染的不消失，以后不能基于数据改变视图
>`componentWillUnmount` 卸载组件之前（一般不用）

![](https://user-gold-cdn.xitu.io/2018/6/29/164495e2ae578b6b?w=855&h=407&f=jpeg&s=39782)

**index.js：**

```
import React from 'react';
import ReactDOM, {render} from 'react-dom';
import PropTypes from 'prop-types';
import 'bootstrap/dist/css/bootstrap.css'

class A extends React.Component {
    static defaultProps = {}; // 第一个执行，属性设置默认值
    constructor() {
        super();
        console.log('1=constructor');
        this.state = {
            n: 1
        }
    }

    componentWillMount() {
        console.log('3=componentWillMount 第一次渲染前', this.refs.HH);
        // 在这里，如果直接setState修改数据（同步的），会把状态信息改变后，然后render和didMount，如果setState是放到一个异步操作中完成（例如：定时器或者从服务器获取数据），也是先执行render和did
        // 然后再执行这个异步操作修改状态，紧接着走修改的流程（这样和放到didMount中没啥区别），所以我们一般吧数据请求放到DID中处理
        // 真实项目中的数据绑定，第一次组件渲染，我们都是绑定的默认属性，第二次才是从服务器获取的数据，有些属性，我们需要根据数据是否存在，判断显示隐藏
    }

    componentDidMount() {
        console.log('4=componentWillMount 第一次渲染后', this.refs.HH);
        //真实项目中，这个阶段一般做如下处理：
        //  1、控制状态信息更改的操作
        //  2、从服务器获取数据，然后修改状态信息，完成数据绑定
        setInterval(() => {
            this.setState({
                n: this.state.n + 1
            })
        }, 5000)
    }

    shouldComponentUpdate(nextProps, nextState) {
        // this.state.n   更新之前的
        console.log('5=shouldComponentUpdate 函数返回true（允许），false（不允许）');
        // return true

        /*在这个钩子函数中，我们获取的state不是最新修改的，而是上一次的state的值
         例如：第一次加载完成后，5000ms后，我们基于setState把n修改为2，但是此处获取的还是1呢
         但是这个有两个参数：
          nextProps:最新修改的属性
          nextState：最新修改的状态
        */

        if (nextState.n > 3) {
            return true
        } else {
            return false
        }
    }

    componentWillUpdate(nextProps, nextState) {
        // this.state.n  也是更新之前的，也有两个参数存储最新的信息
        console.log('6=componentWillUpdate');
    }

    componentDidUpdate() {
        // this.state.n   更新之后的
        // 先render
        console.log('8=componentWillUpdate');
    }

    render() {
        console.log('2=render');
        return <section ref='HH'>
            {this.state.n}
        </section>
    }
}

ReactDOM.render(<main>
    <A></A>
</main>, root);
```
### 七、复合组件的传值
父组件传子组件：
> **基于属性传即可（而且传递是单方向的：只能把信息给儿子，儿子不能直接把信息作为属性传递给父亲）**
> 后期子组件中的信息需要修改：
> 可以让父组件传给子组件的信息发生变化（也就是子组件接收的属性发生变化，子组件会重新渲染 => `componentWillReceiveProps`钩子函数）

子改父：

>类似于这种子改父的操作，我们需要使用一下技巧：
>1、把父组件中一个方法作为属性传递给子组件
>2、在子组件中，把基于属性传递进来的方法，在合适的时候执行（相对于在执行父组件中的方法：而这个方法中完全可以操作父组件的信息）

