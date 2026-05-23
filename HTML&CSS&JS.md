Web标准也称为网页标准，由一系列的标准组成

三个组成部分

- `HTML`：负责网页的结构(页面元素和内容)
- `CSS`：负责网页的表现(页面元素的外观，位置等页面样式，如：颜色，大小等)
- `JavaScript`：负责网页的行为(交互效果)

# HTML，CSS

**HTML：超文本标记语言**

**超文本**：超越了文本的限制，比普通文本更加强大，除了文字信息，还可以定义图片，音频，视频等内容

**标记语言**：由标签构成的语言

- HTML标签都是预定义好的，例如:使用\<a\>展示超链接，使用\<img\>展示图片，\<video\>展示视频
- HTML代码直接在浏览器中运行，HTML标签由浏览器解析



**CSS**：层叠样式表，用于控制页面的样式(表现)





```html
<html>
	<head>
		<title>标题</title>
	</head>
	<body>
		<h1>hello HTML</h1>
	</body>
</html>
```





## 标题排版

```html
<!--文档类型为HTML-->
<!DOCTYPE html>
<html lang="en">
<head>
    <!--字符集为utf-8-->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>新闻标题：11111</title>
</head>
<body>
    <!--
    img标签
    src:图片资源路径
    width:宽度(px,像素；%，相对于父元素的百分比)
    height:高度(px,像素；%，相对于父元素的百分比)

    路径书写方式
    绝对路径：
        1.绝对磁盘路径：E:\study\java_study\code\html入门\img.news_logo.png
                    <img src="E:\study\java_study\code\html入门\img.news_logo.png">
        2.绝对网络路径：
                    https://i2.sinaimg.cn/dy/deco/2012/0613/yocc20120613img01/news_logo.png
                    <img src="https://i2.sinaimg.cn/dy/deco/2012/0613/yocc20120613img01/news_logo.png">
    相对路径
    
    -->
    <img src="https://i2.sinaimg.cn/dy/deco/2012/0613/yocc20120613img01/news_logo.png",width="10px">新浪新闻>正文

    <h1>大标题</h1><!--标题标签-->
    <hr>    <!-- 水平分割线 -->
     2023年。。。。。
    <hr>
</body>
</html>
```



## 标题样式

### **css引入方式**

- 行内样式：写在标签的style属性中(不推荐)
- 内嵌样式：写在style标签中(可以写在页面的任意位置，但通常约定写在head标签中)
- 外联样式：写在一个单独的.css文件中(需要通过link标签在网页中引入)

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>新闻标题：11111</title>
    <!-- 方式2：内联样式 -->
    <!-- <style>
        h1{
            color: red;
        }
    </style> -->
    <!-- 方式3：外联样式 -->
     <link rel="stylesheet" href="css/news.css">
</head>
<body>
    <!-- 方式1:行内样式-->
    <!-- <h1 style="color: red;">大标题</h1> -->
     <h1>大标题</h1>
</body>
```

**颜色表示形式**

| 表示方式       | 表示含义                         | 取值              |
| -------------- | -------------------------------- | ----------------- |
| 关键字         | 预定义的颜色名                   | red,green,blue... |
| rgb表示法      | 红绿蓝三原色，每项取值范围;0~255 | rgb(0,0,0)        |
| 十六进制表示法 | #开头，将数字转化成十六进制表示  | #ff0000           |

**span标签**

`<span>`将没有语义的内容加上标签，方便加style属性之类的



#### **css选择器**

- 元素选择器

```html
<style>
    /*元素选择器：此时所以标签为span的都会加上这个属性*/
    span{
        color:black;
    }

</style>
```

- id选择器

```html
<style>
    /*ID选择器*/
    #time{
        color:blue;
    }
</style>
<span id="time">2023年03月02日</span>
```



- 类选择器

```html
<style>
    /*类选择器*/
    .classname{
        color:green;
    }
</style>
<span class="classname">2023年03月02日</span>
```

优先级：id选择器>类选择器>元素选择器



## 超链接

```html
<a href="" target="">要超链接的内容</a>
href:指定资源访问的url
target:指定在何处打开资源链接
	_self:默认值，在当前页面打开
	_blank:在空白页面打开

e.g
<a href="http://gov.sina.com.cn/" target="_self">新浪新闻</a>
```



## 正文排版

```html
 <!-- 视频 -->
 <video src="video/1.mp4" controls width="500px"></video>

 <!-- 音频 -->
  <audio src="audio/1.mp3" controls></audio>

<!-- 段落-->
<p>
    111
    2222
</p>

<!--文本加粗-->
<b>

</b>
```





## 整体布局

**盒子模型**

盒子模型组成：内容区域(content),内边距区域(padding),边框区域(border),外边距区域(margin)

<img src="C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20260519100441143.png" alt="image-20260519100441143" style="zoom: 50%;" />



网页开发中，会使用div和span这两个没有语义的布局标签

**div标签**

- 一行只能显示一个(独占一行)
- 宽度默认是父元素的宽度，高度由内容撑开
- 可以设置宽高(width,height)

**span标签**

- 一行可以显示多个
- 宽度和高度默认由内容撑开
- 不可以设置宽高

```html
<style>
    #div1{
        width: 400px;/*宽度，400像素，默认是内容展示区域的宽度*/
        height: 300px;/*高度，300像素，默认是内容展示区域的高度*/
        background-color: yellow;
        padding: 30px;/*内边距：30像素*/
        box-sizing: border-box;
        border: 20px solid #ff00ff;/*边框:20px*/
        margin :50px;/*外边框:20px*/
    }
</style>
```





## flex布局

是一种一维的布局模型，flex布局可以为元素提供强大的空间分布和对其能力，**通过给父容器添加flex的相关属性，来控制子元素的位置和排列方式**

<table style="width:100%;border-collapse:collapse;">   <thead>     <tr style="background-color:#43b97c;color:white;text-align:center;">       <th>属性</th>       <th>取值</th>       <th>含义</th>     </tr>   </thead>   <tbody style="background-color:#e6f7ee;">     <tr>       <td>display</td>       <td>flex</td>       <td>使用flex布局</td>     </tr>     <tr>       <td rowspan="2">flex-direction<br>（设置主轴）</td>       <td>row</td>       <td>主轴方向为x轴，水平向右。（默认）</td>     </tr>     <tr>       <td>column</td>       <td>主轴方向为y轴，垂直向下。</td>     </tr>     <tr>       <td rowspan="5">justify-content<br>（子元素在主轴上的<br>对齐方式）</td>       <td>flex-start</td>       <td>从头开始排列</td>     </tr>     <tr>       <td>flex-end</td>       <td>从尾部开始排列</td>     </tr>     <tr>       <td>center</td>       <td>在主轴居中对齐</td>     </tr>     <tr>       <td>space-around</td>       <td>平分剩余空间</td>     </tr>     <tr>       <td>space-between</td>       <td>先两边贴边，再平分剩余空间</td>     </tr>   </tbody> </table>





## 表单标签

在网页中主要负责数据的采集功能，如注册，登录等数据采集

**form表单:**

​	`action`：表单数据提交的url地址

​	`method`：提交方式

​		`get`：默认，表单数据会出现在url后面，形式:/save?name=Tom&age=18	

​			  特点：

​				1.如果表单中包含了隐私数据，get方式并不安全，不推荐使用该方式

​				2.在浏览器中get请求的大小是有限的，不适合提交大数据量表单

​		`post`：表单数据会在消息体/请求体中提交到服务器

​			  特点：

​				1.安全

​				2.请求大小没有限制

```html
<body>

    <form action "/save" method="get">
        姓名： <input type="text" name="name">
        年龄： <input type="text" name="age">
        <input type="submit" value="提交">
    </form>
</body>
```

**表单中的内容要想正常提交，必须要设置一个项为name**



## 表单项

- `<input>`：表单项，通过type属性控制输入形式

<table border="1" cellpadding="10" cellspacing="0" width="50%">   <thead>     <tr style="background-color:#42b878; color:#fff; text-align:center; font-size:24px; font-weight:bold;">       <th>type取值</th>       <th>描述</th>       <th>形式</th>     </tr>   </thead>   <tbody style="background-color:#e7f7ee; font-size:22px;">     <tr>       <td style="color:red; text-align:center;">text</td>       <td>默认值，定义单行的输入字段</td>       <td><input type="text" value="张无忌"></td>     </tr>     <tr>       <td style="color:red; text-align:center;">password</td>       <td>定义密码字段</td>       <td><input type="password" value="123456"></td>     </tr>     <tr>       <td style="color:red; text-align:center;">radio</td>       <td>定义单选按钮</td>       <td>         <input type="radio" name="sex" id="man"><label for="man">男</label>         <input type="radio" name="sex" id="woman"><label for="woman">女</label>       </td>     </tr>     <tr>       <td style="color:red; text-align:center;">checkbox</td>       <td>定义复选框</td>       <td>         <input type="checkbox" id="java"><label for="java">Java</label>         <input type="checkbox" id="game"><label for="game">Game</label>       </td>     </tr>     <tr>       <td style="color:red; text-align:center;">file</td>       <td>定义文件上传按钮</td>       <td><input type="file"></td>     </tr>     <tr>       <td style="color:red; text-align:center;">date/time/datetime-local</td>       <td>定义日期/时间/日期时间</td>       <td>         <input type="date">         <input type="time">         <input type="datetime-local">       </td>     </tr>     <tr>       <td style="color:red; text-align:center;">hidden</td>       <td>定义隐藏域</td>       <td><input type="hidden"></td>     </tr>     <tr>       <td style="color:red; text-align:center;">submit / reset / button</td>       <td>定义提交按钮 / 重置按钮 / 可点击按钮</td>       <td>         <input type="submit" value="提交">         <input type="reset" value="重置">         <input type="button" value="按钮">       </td>     </tr>   </tbody> </table>

- `<select>`：定义下拉列表，`<option>`定义列表项
- `<textarea>`文本域

```html

<table>
<thead>
<tr>
    <th>姓名</th>
    <th>性别</th>
    <th>头像</th>
    <th>职位</th>
    <th>入职日期</th>
    <th>最后操作时间</th>
    <th>操作</th>
</tr>
</thead>
<tbody>
    <tr>
        <td>Tom</td>
        <td>Male</td>
        <td><img src=" " alt="Tom" width="40"></td>
        <td>Teacher</td>
        <td>2024-01-15</td>
        <td>2024-05-20 10:30</td>
        <td>
            <a href="#">Edit</a>
            <a href="#">Delete</a>
        </td>
    </tr>
    <tr>
        <td>Jerry</td>
        <td>Male</td>
        <td><img src="" alt="Jerry" width="40"></td>
        <td>Class Teacher</td>
        <td>2024-02-20</td>
        <td>2024-05-20 11:00</td>
        <td>
            <a href="#">Edit</a>
            <a href="#">Delete</a>
        </td>
    </tr>
    <tr>
        <td>Rose</td>
        <td>Female</td>
        <td><img src="" alt="Rose" width="40"></td>
        <td>Student</td>
        <td>2024-03-10</td>
        <td>2024-05-20 11:30</td>
        <td>
            <a href="#">Edit</a>
            <a href="#">Delete</a>
        </td>
    </tr>
</tbody>

</tbody>
</table>
```







## 表格

`<table>` 定义表格整体

`<thead>`用于定义表格头部

`<tbody>`定义表格中的主体部分

`<tr>`表格的行，可以包裹多个`<td>`

`<td>`表格单元格，可以包裹内容，如果是表头单元格，可以替换为`<th>`

```html
      <table>
        <thead>
        <tr>
            <th>姓名</th>
            <th>性别</th>
            <th>头像</th>
            <th>职位</th>
            <th>入职日期</th>
            <th>最后操作时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
            <tr>
                <td>Tom</td>
                <td>Male</td>
                <td><img src=" " alt="Tom" width="40"></td>
                <td>Teacher</td>
                <td>2024-01-15</td>
                <td>2024-05-20 10:30</td>
                <td>
                    <a href="#">Edit</a>
                    <a href="#">Delete</a>
                </td>
            </tr>
            <tr>
                <td>Jerry</td>
                <td>Male</td>
                <td><img src="" alt="Jerry" width="40"></td>
                <td>Class Teacher</td>
                <td>2024-02-20</td>
                <td>2024-05-20 11:00</td>
                <td>
                    <a href="#">Edit</a>
                    <a href="#">Delete</a>
                </td>
            </tr>
            <tr>
                <td>Rose</td>
                <td>Female</td>
                <td><img src="" alt="Rose" width="40"></td>
                <td>Student</td>
                <td>2024-03-10</td>
                <td>2024-05-20 11:30</td>
                <td>
                    <a href="#">Edit</a>
                    <a href="#">Delete</a>
                </td>
            </tr>
        </tbody>

      </tbody>
```





# JavaScript

是一门跨平台，面向对象的脚本语言，用来控制网页行为，实现页面的交互效果

**组成**

- ECMAScript：规定了JS基础语法核心知识，包括变量，数据类型，流程控制，函数，对象等
- BOM：浏览器对象模型，用于操作浏览器本身，如页面弹窗，地址栏操作，关闭窗口等
- DOM：文档对象模型，用于操作HTML文档，如:改变标签内的内容，改变标签内字体样式等





## 引入方式

**内部脚本**：将JS代码定义在HTML页面中

- Javascript代码必须位于`<script></script>`标签之间
- 在HTML文档中，可以在任意地方，放置任意数量的`<script>`
- 一般会把脚本置于`<body>`元素的底部，可改善速度

```html
<body>
    <script>
        alert('hello world');
    </script>
</body>
```

**外部脚本：**将JS代码定义在外部的JS文件中，然后引入到HTML页面中

```html
<body>
    <script src="js/demo.js"></script>
</body>
```





## 变量/常量

js是弱类型语言，变量可以存放不同类型的值

`let`声明变量

`const`声明常量，一旦声明，常量的值就不可以改变

```html
<script>
    let a=1;
    a='aa';//弱类型
    const pi=3.14;
    pi=3.15//报错   
</script>
```



## 输出语句

- alert()：弹出警告框
- console.log()：写入浏览器控制台
- document.write()：向html的body内输出内容



## 数据类型

基本数据类型和引用数据类型

基本数据类型

number(整数小数）,boolean,null（对象为空）,undefined（变量未初始化）,string（字符串，推荐使用单引号）

typeof获取数据类型



## 函数

```html
//普通函数
function functionName(参数1,参数2,...){	}
function add(a,b){
return a+b;
}
let c=add(1,1);
//匿名函数
let add=(a,b)=>{
	return a+b;
}
alert(add(1,2));
```



## 自定义对象

```html
let 对象名={
	属性名1:属性值1,
	属性名2:属性值2,
	属性名3:属性值3,
	方法名:function(){

	}
	//函数简写
	方法名(){
	
	}
}
```





## JSON

JavaScript对象标记法(JS对象标记法书写的文本)

由于其语法简单，层次结构鲜明，现多用于作为数据载体，在网络中进行数据传输

```json
{
    "name":"Tom",
    "age":20,
    "gender":"男"
}
//json格式的文本所有的key必须使用双引号引起来
```

**json对象的方法**

- JSON.parse：将json字符串转为js对象
- JSON.stringify：将js对象转为json字符串

```html
let person={
    name:'creep',
    age:18,
    gender:'男'
}
alert(JSON.stringify(person));//js对象转为字符串
let personJson='{"name":"creep","age":18}';
alert(JSON.parse(personJson).name);//字符串转成json对象
```





## DOM

文档对象模型，将标记语言的各个组成部分封装为对应的对象

- Document：整个文档对象
- Element：元素对象
- Attribute：属性对象
- Text：文本对象
- Comment：注释对象

<img src="C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20260522100510556.png" alt="image-20260522100510556" style="width:1500px;zoom: 50%;" />



JavaScript通过DOM，就能够对HTML进行操作：

- 改变HTML元素的内容
- 改变HTML元素的样式(CSS)
- 对HTML DOM事件作出反应
- 添加删除HTML元素



### DOM操作

将网页中所有的元素当作对象来处理

**操作步骤**

- 获取要操作的DOM元素对象
- 操作DOM对象的属性或者方法

**获取DOM对象**

- 根据CSS选择器来获取DOM元素，获取匹配到的第一个元素:document.querySelector('选择器')
- 根据CSS选择器来获取DOM元素，获取匹配到的所有元素:document.querySelectorAll('选择器')
  - 得到的是一个NodeList节点集合，是一个伪数组

```html
<body>
    <h1 id="title1"> 1111</h1>
    <h1 id="title2"> 2222</h1>
    <h1 id="title3"> 3333</h1>
    <script>
        //获取DOM对象
        let ob1=document.querySelector('#title1');
        //let ob1=document.querySelector('h1');//获取第一个h1标签
        //调用DOM对象中属性或方法
        ob1.innerHTML='修改后的文本';

        let obs=document.querySelectorAll('h1');
        obs[0].innerHTML='修改后的文本2';
    </script>
</body>
```



## 事件监听

**事件：**HTML事件是发生在HTML元素上的事情

- 按钮被点击
- 鼠标移动到元素上
- 按下键盘按键

**事件监听：**JavaScript可以在事件触发时，就立即调用一个函数作出响应

```html
事件源.addEventListener('事件类型',事件触发执行的函数);
```

```html
<input id="btn" type="button" value="点我一下试试">
<script>
    //事件监听-addEventListenr可多次绑定同一个事件
    document.querySelector('#btn').addEventListener('click',()=>{
        alert('试试就试试');
    })
    //事件绑定:-onclick 如果多次绑定同一个事件，覆盖
    document.querySelector('#btn').onclick=()=>{
        alert('试试就试试2');
    }
</script>
```

### **常见事件**

- 鼠标事件
  - click：鼠标点击
  - mouseenter：鼠标移入
  - mouseleave：鼠标移出
- 键盘事件
  - keydown：键盘按下触发
  - keyup：键盘抬起触发
- 焦点事件
  - focus：获得焦点触发
  - blur：失去焦点触发
- 表单事件
  - input：用户输入时触发
  - submit：表单提交时触发







## Vue

Vue是一款用于构建用户界面的渐进式的JavaScript框架

**流程**

- 引入Vue模块(官方模块)
- 创建Vue程序应用实例，控制试图的元素
- 准备元素(div),被Vue控制

```html
    <div id="app">
        <h1>{{message}}</h1>
    </div>

    <script type="module">
        import {createApp} from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js';
        createApp({
            data(){
                return{
                    message:'hello Vue'
                }
            }
        }).mount('#app');
    </script>
```





### v-for指令

作用：列表渲染，遍历容器的元素或者对象的属性

```html
<tr v-for="(item,index) in items":key="item.id">{{item}}</tr>
```

- items：遍历的数组
- item：遍历的元素
- index：索引/下标，从0开始，可以省略，省略index语法：v-for=item in items

**key**

- 给元素添加的唯一标识，便于vue进行列表项的正确排序复用，提升渲染性能
- 推荐使用id作为key





### v-bind

**作用**：动态为HTML标签绑定属性值，如设置href，src，style样式等

```html
v-bind:属性名="属性值"
//<img v-bind:src="item.image">
//简化
属性名="属性值"
```

**动态的为标签的属性绑定值，不能使用插值表达式，得使用v-bind指令，且绑定的数据，必须在data中定义**





### v-if &v-show

这两类指令，都是用来控制元素的显示和隐藏的

#### v-if

- ```html
  v-if="表达式",表达式值为true显示，false隐藏
  ```

- 原理：基于条件判断，来控制创建或移除元素节点(条件渲染)

- 场景：要么显示，要么不显示，不频繁切换的场景

- 可以配合v-else-if/v-else 进行链式调用

#### v-show

- ```html
  v-show="表达式",表达式为true显示，false隐藏
  ```

- 原理：基于css样式display来控制显示和隐藏

- 场景：频繁切换显示隐藏的场景

```html
<td>
<span v-if="e.job==1">班主任</span>
<span v-else-if="e.job==2">讲师</span>
<span v-else-if="e.job==3">学工主管</span>
<span v-else-if="e.job==4">校验主管</span>
<span v-else>咨询师</span>
</td>
```





### v-model

**作用**：在表单元素上使用，双向数据绑定，可以方便的获取或设置表单项数据

```html
createApp({
    data(){
        return{
            searchForm:{
                name:'',
                gender:'',
                job:''
            }
        }
    }
}).mount('#container');

<input type="text"  name="name"  v-model="searchForm.name">
```





### v-on

**作用：**为html标签绑定事件(添加事件监听)

```html
v-on:事件名="方法名"
//可以简写为 @事件名="..."
```

方法名需要在methods中定义

```html
<button type="button" v-on:click="函数名"></button>
createApp({
    data(){
        //...
    },
    methods:{
        函数名(){
            ...
        }
    },
}).mount('#container');
```





## Ajax

异步的JavaScript和XML

**作用**：

- 数据交换，通过Ajax可以给服务器发送请求，并获取服务器响应的数据
- 异步交互，可以在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术



#### Axios

Axios对原生态Ajax进行了封装，简化书写

**步骤**

- 引入Axios的js文件(参照官网)
- 使用Axios发送请求，并获取响应结果

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
//GET请求
document.querySelector('#getData').onclick = function() {
  axios({
    url:'https://mock.apifox.cn/m1/3083103-0-default/emps/list',
    method:'get'
  }).then(function(res) {
    console.log(res.data);
  }).catch(function(err) {
    console.log(err);
  })
}

//POST请求
document.querySelector('#postData').onclick = function() {
  axios({
    url:'https://mock.apifox.cn/m1/3083103-0-default/emps/update',
    method:'post'
  }).then(function(res) {
    console.log(res.data);
  }).catch(function(err) {
    console.log(err);
  })
}
</script>
```

- method：请求方式，GET/POST
- url：请求路径
- data：请求数据（POST）
- params：发送请求时携带的url参数





**简化写法**

```javascript
axios.get("https://mock.apifox.cn/m1/3083103-0-default/emps/list").then(result => {
    console.log(result.data);
})
axios.post("https://mock.apifox.cn/m1/3083103-0-default/emps/update","id=1").then(result => {
    console.log(result.data);
})
```



本来是异步操作，可以通过加上async和await关键字让异步变成同步操作

```javascript
async search() {
//基于axios发送异步请求，请求https://web-server.itheima.net/emps/list，根据条件查询员工列表
const result = await axios.get(`https://web-server.itheima.net/emps/list?name=${this.searchForm.name}&gender=${this.searchForm.gender}&job=${this.searchForm.job}`);
this.empList = result.data.data;
},
```









## Vue声明周期

vue的生命周期：指的是vue对象从创建到销毁的过程。

vue的生命周期包含8个阶段：每触发一个生命周期事件，会自动执行一个生命周期方法，这些生命周期方法也被称为钩子方法。其完整的生命周期如下图所示：

| 状态          | 阶段周期 |
| ------------- | -------- |
| beforeCreate  | 创建前   |
| created       | 创建后   |
| beforeMount   | 挂载前   |
| mounted       | 挂载完成 |
| beforeUpdate  | 更新前   |
| updated       | 更新后   |
| beforeDestroy | 销毁前   |
| destroyed     | 销毁后   |
