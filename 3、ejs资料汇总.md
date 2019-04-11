跳槽后，自己不仅仅要写react、vue项目，还要维护公司现存的用ejs+JQuery开发的老项目，尤其是ejs，在搜索引擎中收录的学习文档和大佬们的解决方案都差不多是一到两年前的了，总结出此文给还在维护这些语言开发的项目的新手们。
### 一、什么是ejs？
EJS 是一套简单的模板语言，用来从JSON数据中生成HTML字符串。
### 二、之前为什么要使用EJS？
vue和react没有出现之前，前端绑定数据除了字符串拼接、模版绑定（handlebar、ejs）还有后端生成vm、jsp（使用JSTL和Velocity语法）

（1）字符串拼接
```javascript
var html = "<h1>"+data.title+"</h1>"
html += "<ul>"
for(var i=0; i<data.supplies.length; i++) {
    html += "<li><a href='supplies/"+data.supplies[i]+"'>"
    html += data.supplies[i]+"</a></li>"
}
html += "</ul>"
```
如果页面结构复杂，你需要拼接好多，维护也不简单<br>
其实ES6的模版字符串也能简化拼接，但是老项目大多不能编译ES6

（2）EJS

为了解决这些问题，大佬开发了各种模版语言，ejs就是其中一种。<br>
创建一个EJS模板,命名为cleaning.ejs文件，内容如下：
```javascript
<h1><%=title %></h1>
 <ul>  
  <% for (var i=0; i<supplies.length; i++) { %>       
    <li> 
      <a href= 'supplies/<%=supplies[i] %>'><%= supplies[i] %></a>        
    </li>   
  <% } %>
 </ul>
```
完整内容如下：
```javascript
<html>
 <head>
 <script type="text/javascript" src = "/js/ejs.js"></script>
 <script type ="text/javascript" >
   function myfunction(){
    var data='{"title":"cleaning","supplies":["mop","broom","duster"]}'
    var html = new EJS({url: '/js/tmpl/cleaning.ejs'}).render(JSON.parse(data));
    //JSON.parse(data) 把JSON字符串解析为原生的javascript值。
     alert(html);     
    document.getElementById("div1").innerHTML=html;
  }
 </script>
 </head>
 <body>
     <button onclick = "myfunction()" >点击</button>
     < div id = "div1" ></div >
 </body>
 <html>
```
或者：
```
<script src="ejs.js"></script>
<script>
  var people = ['geddy', 'neil', 'alex'],
      html = ejs.render('<%= people.join(", "); %>', {people: people});
</script>
```
#### （2）项目中
```
<script type="text/template" id="searchContTemplate">
    <div class="searchCont">	
	<%if(code==1 && data ){%>
	    <div class="second-title"><label>XXXX：<%=data.custName%></label><label class="ml">客源编号：<span class="keCode"><%=data.id%></span></label></div>
	    <div class="cont">
           <% var list=data.showing%>
		    <%for(var i=0;i<list.length;i++){%>
		         <div class="item">
		            <img src="<%=list[i].picUrl%>" alt="">
		            <div class="info">
		               <label>	
	                       <h2><%=list[i].resblockName%></h2>
	                       <p class="foot">XXX <%=list[i].houseCode%></p>
                       </label>
                       <input type="radio" class="" name="houseCode" value="<%=list[i].houseCode%>">
		            </div>
		        </div>
		    <%}%>
	    </div>
	    <div class="control"><button class="btn-cancel actClose">取消</button><button class="btn-green btn-green actSelectFang">确认</button></div>
	    <%}else{%>
	     <div>暂无数据</div>
	    <%}%>
  </div>		    
</script>
```
数据填充：

```javascript
var template = $.template($("#searchContTemplate").html());
var result = template.render(data);
$(".fangKeSearch").append(result);
```
#### （3）npm
```
npm install ejs
```
```
var ejs = require('ejs'),
    people = ['geddy', 'neil', 'alex'],
    html = ejs.render('<%= people.join(", "); %>', {people: people});
```
剩下的是官方文档中的内容（备份一下，我也粘上来了，可以不看）
### 四、参数
```
cache 缓存编译后的函数，需要提供 filename
filename 被 cache 参数用做键值，同时也用于 include 语句
context 函数执行时的上下文环境
compileDebug 当为 false 时不编译调试语句
client 返回独立的编译后的函数
delimiter 放在角括号中的字符，用于标记标签的开与闭
debug 将生成的函数体输出
_with 是否使用 with() {} 结构。如果为 false，所有局部数据将存储在 locals 对象上。
localsName 如果不使用 with ，localsName 将作为存储局部变量的对象的名称。默认名称是 locals
rmWhitespace 删除所有可安全删除的空白字符，包括开始与结尾处的空格。对于所有标签来说，它提供了一个更安全版本的 -%> (在一行的中间并不会剔除标签后面的换行符)。
escape 为 <%= 结构设置对应的转义（escape）函数。它被用于输出结果以及在生成的客户端函数中通过 .toString() 输出。(默认转义 XML)。
```
### 五、标签含义
`<%` '脚本' 标签，用于流程控制，无输出。
`<%_` 删除其前面的空格符
`<%=` 输出数据到模板（输出是转义 HTML 标签）
`<%-` 输出非转义的数据到模板
`<%#` 注释标签，不执行、不输出内容
`<%%` 输出字符串 '<%'
`%>` 一般结束标签
`-%>` 删除紧随其后的换行符
`_%>` 将结束标签后面的空格符删除

### 六、包含include
通过 `include` 指令将相对于模板路径中的模板片段包含进来。(需要提供 '`filename`' 参数。) 例如，如果存在 "`./views/users.ejs`" 和 "`./views/user/show.ejs`" 两个模板文件，你可以通过 `<%- include('user/show'); %>` 代码包含后者。

你可能需要能够输出原始内容的标签 (`<%-`) 用于 `include` 指令，避免对输出的 HTML 代码做转义处理。
```javascript
<ul>
  <% users.forEach(function(user){ %>
    <%- include('user/show', {user: user}); %>
  <% }); %>
</ul>
```
### 六、自定义分隔符
可针对单个模板或全局使用自定义分隔符：
```
var ejs = require('ejs'),
    users = ['geddy', 'neil', 'alex'];

// 单个模板文件
ejs.render('<?= users.join(" | "); ?>', {users: users},
    {delimiter: '?'});
// => 'geddy | neil | alex'

// 全局
ejs.delimiter = '$';
ejs.render('<$= users.join(" | "); $>', {users: users});
// => 'geddy | neil | alex'
```

未完：边开发边完善

PS：发布的时候，对应的标签都没有，EJS要凉了吗？

中文文档：[EJS文档](https://ejs.bootcss.com/)