---
title: Vue
date: 2024-11-18 09:59:32
auther: 小狼
summary: 前端三件套+Vue简要笔记整理
categories: 前端
img: /medias/featureimages/16.jpg
tags:
  - 框架
  - 前端
---

# Vue

## 前端

- web里的网页里 有 三门基础的语言 HTML CSS JavaScript/js
- HTML用于定义网页的基本内容
- CSS样式用于调整网页内容的样式(外观相关的)
- js 用于给网页内容 添加 功能

### HTML

HTML（超文本标记语言）是一种用于创建网页标记语言。HTML使用标签来定义网页中的元素，如标题、段落、链接、图像等。它是一种静态语言，主要负责网页的结构和语义化。

#### 网页基本信息

```html
<!-- DOCTYPE 文本类型，告诉浏览器使用的是什么规范-->
<!DOCTYPE html>

<html lang="en">
    <!-- head 网页头部 -->
    <head>
        <!-- meta 描述性标签，可描述一些网站信息：比如编码、关键字、间接叙述-->
        <meta charset="UTF-8">
        <meta name="keywords" content="前端开始">
        <meta name="description" content="这是三件套的开始">
        <!-- title 打开网站后看到的网站名称   -->
        <title>First HTML</title>
    </head>

    <!-- 主体 内部是可在网页页面上显示的内容-->
    <body>
        Hello，HTML
    </body>

</html>
```

#### 基本标签

```html
<!--标题标签-->
<h1>复习前端</h1>
<h2>复习前端</h2>
<h6>复习前端</h6>
<!--段落标签，每个标签自成一段-->
<p>复习前端</p>
<p>复习前端</p>
<!--换行标签，只是换行，多行成一段-->
复习前端<br>
复习前端<br>
<!--水平线标签-->
<hr>
<hr>
<!--字体样式标签-->
粗体：<strong>复习前端</strong>
斜体：<em>复习前端</em>
<!--特殊格式-->
空&nbsp;格
```

**块级标签,简称块**：默认在页面布局里,自己独占一行。

* CSS盒子模型属性里的 宽高,内填充,外边框,边框 都可以单独控制且有效果

* 常用的块级元素有 h1~h6 p ul li ol table form div…

**行级标签/元素**：默认的在页面布局里,自己会跟其他行级标签挤在一行,除非这一行挤不下,才会换行

* CSS盒子模型属性里的 宽高,内填充,外边框,边框 有一部分是可以控制且有效果的
* 常用的行级元素有 a b span font img input select label…

#### 特殊标签

```html
<!--1.图片标签-->
<img src="../resources/image/图片一.jpg" alt="风景" content="落日" title="前端复习">
alt图片加载失败时显式的文字，title鼠标悬停到图片显示的文字

<!--2.链接标签-->
<a href="HelloHTML.html" target="_blank">点击跳转，文字跳转</a>
<a href="HelloHTML.html">
    <img src="../resources/image/图片二.jpg" alt="风景" title="前端复习" height="300" width="300">
</a>

<!--锚链接，定义一个锚标记，然后跳转到锚标记-->
<a href="top">顶部</a>
<a href="#top">回到顶部</a>

<!--邮件链接-->
<a href="mailto:2869861273@qq.com">点击发送邮件</a>
</body>

<!--3.列表标签-->
<!--有序列表-->
<ol>
    <li>前端复习1</li>
    <li>前端复习2</li>
    <li>前端复习3</li>
</ol>
<!--无序列表，不是顺序杂乱，而是没有序号标明顺序-->
<ul>
    <li>前端复习1</li>
    <li>前端复习2</li>
    <li>前端复习3</li>
</ul>
<!--自定义列表,dt标题，dd内容-->
<dl>
    <dt>前端复习</dt>
    <dd>前端复习1</dd>
    <dd>前端复习1</dd>
    <dd>前端复习1</dd>
</dl>

<!--4.表格标签，tr表格里的行，td表格里的列-->
table 表格 默认不显示边框 border属性可以调整边框
th 表头单元格 比普通单元格 多了 加粗字体,默认居中
<table border="1px">
    <tr>
        <!-- 跨列合并，保留最左单元格，添加属性 colspan -->
        <td colspan="3">1</td>
    </tr>
    <tr>
        <!-- 跨行合并，保留最上单元格，添加属性 rowspan -->
        <td rowspan="2">复习</td>
        <td>前端复习</td>
        <td>前端复习</td>
        <!-- 当设置了跨行和跨列时，后面所被影响的表格需要去掉从而对齐  -->
    </tr>
    <tr>
        <td>前端复习</td>
        <td>前端复习</td>
    </tr>
    
    <!--5.音视频标签,controls显示进度条 autoplay设置自动播放-->
    <video src="" controls autoplay></video>
    <audio src="" controls autoplay></audio>
    
    <!--6.iframe内联标签，即在一个网页里还有一个内联框,内联框里是另一个网址-->
    <iframe src="https://www.baidu.com" frameborder="0" width="500" height="500"></iframe>
</table>
```

#### form表单

* 表单是用来收集用户的不同输入的数据,然后发送给"服务器程序"
* 表单form标签,内部可以定义很多不同形式的"表单组件".
* 表单组件有的表现为单行文本框,密码框,按钮,复选框,下拉菜单
* form标签有method属性,设置 本表单的提交方式,默认值为get 可以设置为post
* form标签有action属性,设置 本表单提交到的路径,通常是"服务器程序"的路径
* form标签有enctype属性,设置 当表单里有文件上传时,表单的特殊编码形式

```html
<!--7.表单 -->
<form action="HelloHTML.html" method="get" enctype="multipart/form-data">
    <!-- name的值就是提交的时候的键 组件里用户输入的数据,就是提交的时候 键对应的值 -->
    <!-- value 可以设置默认值 -->
    <!-- placeholder 占位符,不输入的时候一个提示 -->
    <!-- size 输入框的长度 -->
    <!-- maxlength 能输入的字符的最大个数 -->
    <!-- required 表示必须填写,否则表单无法提交,属性名和值相同的,这样的属性 可以直接写属性 -->
    <!-- pattern 设置 正则表达式 -->
    <!-- 提交按钮 HTML中已经定义好的 可以提交表单的按钮,把表单里的数据发送出去 -->
    <input type="text" name="username" placeholder="输入用户名" value="name" size="30"
           required="required" pattern="^[a-zA-Z]\w{3,7}$" >
    
    <!-- 单选按钮 必须提供相同的name属性 才能成为互斥的一组
    <label>Gender:</label>
    <input type="radio" name="gender" value="1" checked/><label>Female</label><img src="img/logo.png"/>
    <input type="radio" name="gender" value="2"/><label>Male</label><img src="img/logo.png"/>
    <br/>
    input 单标签 它的值需要使用value来定义.后面的label/文本/图片 仅仅和它是排版上一起
    单选按钮 默认选择的属性 checked -->
    
    <!-- 图片形式的提交按钮 -->
    <input type="image" src="img/logo.png" width="50px" height="50px"/>
    <input type="button" value="普通按钮"/> <!-- js添加功能 -->
    <!-- 按钮这里 还有一个特殊的button双标签 根据type属性的不同 有不同的效果 -->
    <br/>
    
    <button type="button">普通按钮</button>
    <button type="submit">提交按钮:有功能</button>
    <button type="reset">重置按钮:有功能</button>

    <!--单选框 name表示组，只有在同一组的才能选择其一   -->
    男<input type="radio" value="girl" name="sex">
    女<input type="radio" value="boy" name="sex">
    <!--多选框 checked默认选中 -->
    <p>爱好：
        游戏<input type="checkbox" value="play" name="hobby" checked>
        舞蹈<input type="checkbox" value="dance" name="hobby">
        吃<input type="checkbox" value="eat" name="hobby">
    </p>

    <input type="hidden" name="secret" value="秘密数据"/>
    <!-- 隐藏域 用户看不到,但是可以跟随表单一起提交的数据,一般是程序员设置在此的一个数据 -->
    <label for="birthdayInput">Birthday:</label>
    <!-- 日期，邮件等 -->
    <input type="date" name="birthday" id="birthdayInput">
    <br/>
    <label for="emailInput">Email:</label>
    <input type="email" name="email" id="emailInput" required/>
    <br/>
    <input type="color" name="color">
    <input type="week" name="week">
    <input type="time" name="time">

    <!-- 有文件上传时,必须将form的enctype 属性 改为enctype="multipart/form-data"(多组件表单数据)
      并且 method 必须改为post,这里没有改 因为 并不能进行真正的上传
     -->
    <label for="photoChoose">Photo:</label>
    <input type="file" name="photo" id="photoChoose"/>
    <br/>

    <!--8.下拉框 selected默认选择-->
    <p>下拉框：
        <select name="列表名称" >
            <option value="china" selected disabled>中国</option>
            <option value="us">英国</option>
            <option value="ufo">美国</option>
        </select>
    </p>
    <!--9.文本域，cols，rows行列长度-->
    <p>文本域：
        <textarea name="textarea" cols="30" rows="10">文本框</textarea>
    </p>
</form>
```

### CSS

CSS（层叠样式表）是一种用于描述网页样式和布局的样式表语言。它与HTML配合使用，负责网页的外观和样式。通过使用CSS，可以控制元素的大小、颜色、字体、布局，实现网页的美观效果和排版。

#### 导入方式

```html
<!-- 1.内部样式-->
<style>
    h1{
        color: red;
    }
</style>

<!-- 2.外部样式：链接外部样式表是指通过HTML的link链接标签，建立样式文件和网页的关联。-->
<head>
	<link rel="stylesheet" href="css/style.css">
</head>

<body>
    <!-- 3.行内样式-->
    <h1 color="green">Hello</h1>
    <p style="color:red; font-size:30px; font-family:黑体;">
</body>
```

#### 基本选择器

```html
<!--     1.标签选择器，选择所有同一标签     -->
<style type="text/css">
	标签名 {
		属性1：属性值1;
		属性2：属性值2;
	}
    h1 {
         color: red;
     }
</style>

<!--    2.类选择器，需先在标签里定义一个任意类属性，可多个标签定义同一个类名，然后在css中.类名-->
<style>
    .类名 {
        属性1：属性值1;
    }
    .liy{
        color: red;
    }
</style>
<!--    使用类样式    -->
<标签名 class="类名"> 标签内容 </标签名>
<h1 class="liy">类选择器</h1>
<h2 class="liy">类选择器</h2>

<!--  3.id选择器，同样先定义一个id，再在css中#id选择，但一个id只能定义一个标签  -->
<style type="text/css">
	#ID标识名 {
		属性1：属性值1;
		属性2：属性值2;
	}
    #gu{
        color: red;
    }
</style>
<h1 id="gu">id选择器</h1>

<!--  4.通配符选择器，查找页面所有标签，设置相同样式  -->
* {
  color: red;
}
```

#### 高级选择器

```html
<style>
    body p{   选中的是body下面的所有p标签 -- 后代选择器
        background: red;
    }
</style>

<style>
    body > p{   选中的是body下面的一代p标签 -- 子代选择器
        background: red;
    }
</style>

<style>
    p,.red,#header {   组合（并集）选择器，逗号分隔
	color:red;
	font-size:12px;
	}
</style>

<style>
    .active+p {   选的是active标签之后的一个p标签  --  相邻兄弟选择器
        background: red;
    }
</style>

<style>
    .active~p {   选的是激活标签之后的所有p类型标签
        background: red;
    }
</style>

<style>
    /* ：hover 鼠标悬浮时  :active 鼠标点击但未松开时  :focus 获取光标焦点时*/
    h1:hover,p:hover{    伪类选择器：伪类表示元素状态，选中元素的某个状态设置样式。
        background-color: azure;
    }
    h1:active{
        color: cornflowerblue;
    }
    
    /* :first-child 选择作为第一个子元素的 */
    tr:first-child:hover{
        background-color: aquamarine;
    }
    /*
    这里是对tr进行 筛选, 选出所有tr中 作为别人的第一个子元素的tr
    并不是选取 tr内部的第一个孩子
     */

</style>
```

#### 属性选择器

```css
/* 首先搭建标签并设置标签的格式 */
<p class="demo">
 <a href="www.baidu.com" class="links item first" id="first">1</a>
 <a href="" class="links item active" target="_blank" title="test">2</a>
 <a href="resources/image/图片一.jpg" class="links item" >3</a>
 <a href="abc" class="links item">4</a>
 <a href="/a.pdf" class="links item">5</a>
 <a href="/abc.pdf" class="links item">6</a>
 <a href="abcd.docx" class="links item">7</a>
 <a href="abc.docx" class="links item">8</a>
 </p>
 <style>
     .demo a{
         float: left;
         display: block;
         height: 50px;
         width: 50px;
         border-radius: 10px;
         background: #2700ff;
         text-align: center;
         color: gainsboro;
         text-decoration: none;
         margin-right: 5px;
         font: bold 20px/50px Arial;
     }
 </style>

/*  在style中进行选择，属性选择器格式就是标签+[属性]  */
/*    1.选择存在id的元素*/
 a[id] {
     background: red;
 }

 /*    2.选择id为first的标签*/
 a[id="first"] {
     color: black;
 }

 /*    3.选中class里有links的标签*/
 a[class*="links"] {
     background: bisque;
 }

 /*    4.特定的开头和结尾  */
 a[href^=w] {
     background: cornflowerblue;
 }

 a[href$=x] {
     background: red;
 }
```

#### 文字控制

```css
p{
    font-family: 华文行楷;   /* 字体类型  */
    font-size: 16px;	/* 字体大小  */
    font-style: oblique;	/* 字体样式：italic斜体字，oblique倾斜的文字 */
    font-weight: 900;	/* 字体粗细  */
}
span{
    font-size: 2rem;
    line-height:25px;  /* 设置行高（即行间距），常用取值为 25px、28px */
    /* 设置元素中文本的水平对齐方式，继承给子级 */
    text-align: right;
}  

<style type="text/css">
    #p1 {
        text-indent: 2em;
    }

    #p1 > a {
        text-decoration: none;	/* 设置文本修饰， 常用的取值为underline（下划线）、none  */
        color: green;
        font-weight: bold;
    }

    #p1 > a:hover {
        text-decoration: underline;
        color: gold;
        background-color: green;
    }

    .pp2 {
        text-transform: capitalize;
        word-spacing: 2em;
        text-align: center;		/*  设置对齐方式，常用的取值为left、right 以及 center  */
    }

    .pp3 {
        letter-spacing: 1em;	/* 设置字符间距，常用的取值为3px、8px */
    }

    ul {
        /*list-style-position: inside;*/
        /*list-style-type: decimal;*/
        /*list-style-type: none;*/
        /*list-style-image: url("img/logo.png");*/
        /* 列表在设置时,一般会直接把列表项标志设置为none */
    }

    table {
        border-collapse: collapse;
        /*border-spacing: 10px 15px;*/
        /*caption-side: bottom;*/
       /* empty-cells: hide;*/

        /* 列宽度算法 */
        /*table-layout: fixed;*/
    }
    button{
        font-size: 100px;
        /*background-color: transparent;*//* 透明 */
    }
    button:hover{
        /*outline-style: solid;
        outline-width: 2px;
        outline-color: red;*/
        outline: green double 3px;
    }
</style>
```

#### CSS特性

CSS特性：化简代码 / 定位问题，并解决问题

* 继承性：子级默认继承父级的**文字控制属性**。 如果标签有默认文字样式会继承失败。
* 层叠性：相同属性后面的覆盖前面的，不同属性叠加。
* 优先级：当一个标签**使用了多种选择器时**，基于不同种类的选择器的**匹配规则**。

**基础选择器**

规则：选择器**优先级高的样式生效**。

公式：**通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < !important**

​           **（选中标签的范围越大，优先级越低）**

**复合选择器-叠加**

叠加计算：如果是复合选择器，则需要**权重叠加**计算。

公式：（每一级之间不存在进位）

<img src="Vue\1680319646205-1732170256083.png" alt="1680319646205" style="zoom:67%;" />

规则：

* 从左向右依次比较选个数，同一级个数多的优先级高，如果个数相同，则向后比较
* **!important 权重最高**
* 继承权重最低

#### 显示设置

display 用于设置行内元素的排列。display方向不可以控制。

float浮动起来的话会脱离标准文档流，所以要解决父级边框塌陷的问题

```css
display: block;	/* 块级 */
display: inline-block; /* 行内块 一行可以显示多个 */
display: inline; /* 行内 */

float: left; /* 独立于底层，设置浮动 */
float: right;
clear: right; /* 右侧不允许有浮动元素 */
clear: both; /* 两侧都不允许有浮动元素 */
clear: right;
clear: none;

#d1{
    width: 200px;
    height: 200px;
    border: 2px solid dimgray;
    /* overflow 调整内容溢出 */
    /*overflow: hidden; *//*隐藏 */
    /*overflow: scroll;*//* 滚动条*/
    overflow: auto;/* 自动添加滚动条 */
}
```

盒子模型：即边距margin，边框border，填充padding，和实际内容。

外边距设置居中：margin: auto;（前提该标签需要在一个块元素内，即要有边界）

```css
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">
        .div01{
            width: 200px;/* width,height仅仅设置内容区域 */
            height: 200px;
            padding: 2em 4em;/* padding是简写,简写了四个方向 */
            /* 如果是给1个值 那就是 4个方向都设置 */
            /* 如果给2个值 第一个值是 上下 第二个值是 左右 */
            /* 如果给3个值 第一个值是 上 第二个值 左右 第三个值 下 */
            /* 如果给4个值 分别是 上 右 下 左 顺时针 */
            border: 2px solid red;
        }

        span{
            width: 200px;
            height: 200px;
            /* 行级元素的 宽高属性 无效,根据内容的大小自动调整的 */
        }
        #span2{
            /*border: 10px solid red;*//* 边框可以调整左右,上下不建议调整 */
            /*padding: 20px;*//* 内填充可以调整左右,上下不建议调整 */
            /*margin: 20px;*//* 外边距可以调整左右,上下不建议调整 */
            display: block;
            width: 200px;
            height: 200px;
            border: 2px solid purple;
        }
        .div01:first-child{
            border:2px solid blue;
            margin:20px;
            /* padding margin border 都可以写 xxx-方向 */
        }
    </style>
</head>

<body>
   <!-- div 块级标签 没有任何显示效果,需要结合css盒子模型属性的调整 -->
   <div class="div01" style="display: none">
       这是内容!
   </div>
   <div class="div01" style="display: inline">
       这是内容!
   </div>
   <!-- 盒子模型/框模型 在 行级元素里的部分 不建议调整 -->
   <span id="span1">span1</span><span id="span2">span2</span><span id="span3">span3</span>
</body>
```

#### 定位

文档流：默认的摆放形式，行级和行级在一行，实在放不下就换行，块级独占一行

* 相对定位relative：相对于 自身加载的原始位置 的 定位 原来的位置是被保留的
* 绝对定位absolute：相对于 已经定位的父元素 的 定位 原来的位置不被保留.
* 固定定位fixed：相对于 视窗(浏览器窗口) 原来的位置不被保留.

```css
#move{
    width: 400px;
    height: 200px;
    background-color: gold;
    /* 固定定位 */
    position: fixed;
    top: 200px;
    left: 400px;
    z-index: -1;
}
#outer{
    width: 200px;
    height: 200px;
    background-color: blue;
    position: relative;/* 相对定位 */
    left: 100px; /* 定位之后的位置 左边 100px 是原来的位置 */
    top: 100px; /* 定位之后的位置 上边100px 是原来的位置 */
    z-index: -1;
}
#outer2{
    width: 200px;
    height: 200px;
    background-color: aquamarine;
    position: absolute;/* 绝对定位 父亲是body */
    left: 200px;
    top: 100px;
    z-index: -2;
}
```

### JavaScript

JavaScript是一种基于**对象和事件驱动**的脚本语言，作用是给网页添加交互功能和动态效果。它可以操作网页元素、响应用户操作、发送网络请求、处理数据等。JavaScript使得网页具有更高级的交互性和动态性，可以实现表单验证、页面动画、实时数据更新等。

html中使用js有三种,标签内,内部,外部 使用js一般是两种,也就是内部和外部

标签内：

```html
<a href="javascript:alert('这是js的信息弹框!这样的用法几乎不使用')">点我弹出一个信息框</a>
```

内部：

```html
<head>
...
  <script type="text/javascript">
        /* 脚本 */
       /* alert("js的弹框000");*/
        //js 里函数 就是 java里的方法
        function show() {
            alert('show方法里的弹框!')
        }
    </script>
...
<head>

<div onclick="show()">
    通过在标签上的 事件动作 on在click点击 的时候 执行 js函数 show()
</div>
```

外部：

把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码。外部 JavaScript 文件的文件扩展名是 .js。如需使用外部文件，请在 \<script\> 标签的 "src" 属性中设置该 .js 文件：

```html
<!DOCTYPE html>
<html>
    <body>
        <script src="myScript.js"></script>
    </body>
</html>
```

#### JS输出

JavaScript 可以通过不同的方式来输出数据：

- 使用 **window.alert()** 弹出警告框。
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素：如需从 JavaScript 访问某个 HTML 元素，您可以使用 document.getElementById(*id*) 方法。请使用 "id" 属性来标识 HTML 元素，并 innerHTML 来获取或插入元素内容
- 使用 **console.log()** 写入到浏览器的控制台。

#### 语法

在编程语言中，一般固定值称为字面量，如 3.14。

```javascript
3.14  1001  123e5
"John Doe"  'John Doe'
[40, 100, 1, 5, 25, 10]
{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}
function myFunction(a, b) { return a * b;}
// 对象寻址方式
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
name = person.lastname;
name = person["lastname"];
```

JavaScript 使用关键字 **var** 来定义变量， 使用等号来为变量赋值。变量可以通过变量名访问。在指令式语言中，变量通常是可变的。字面量是一个恒定的值。

- js里语句最后的分号可加可不加
- 使用关键字 var let 可以声明变量,const声明常量
  - var声明的变量 为 js顶级对象window的属性,为全局变量
  - let声明的变量 为 局部变量

**值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、空（Null）、未定义（Undefined）、Symbol。

**引用数据类型（对象类型）**：对象(Object)、数组(Array)、函数(Function)，还有两个特殊的对象：正则（RegExp）和日期（Date）。

流程控制

```javascript
let arr = [10, 99, 77, 35, 28, 101];
//遍历
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
console.log('=============');
for (let i of arr) {
    console.log(i);
}//iter
console.log('============');
for (let i in arr) {
    console.log(arr[i]);
}//forin
console.log('-------------')


//对象
let stu = {
    name: 'jacky',
    age: 10,
    gender: '男',
    address: "长大",
    study: function () {
        alert("学习学习")
    }
}
//for-in可以去遍历对象,遍历对象的时候,对象类似一个Map集合  let变量 是 键
for (let stuKey in stu) {
    console.log(stu[stuKey]);
}
// 错误处理
try {
    console.log(person.name + person.age);//这里有错误
}catch (e) {
    console.log(e);
}
```

Map，Set：ES6新特性  Map,同java里的Map  js里只有这两个数组和集合

```javascript
var map = new Map([["tom",10],["jack",20],["okk",30]])
map.get("tom")
10
map.set("ggz",8)
Map(4) {'tom' => 10, 'jack' => 20, 'okk' => 30, 'ggz' => 8}
idea书写
var map = new Map([["tom",10],["jack",20],["okk",30]])
var age1 = map.get("tom")
console.log(age1);
map.set("qqz",10);
console.log(map);
var set = new Set([1,2,3,3,3,3]);  是一个无序集合
set.add(5);  添加
set.has(3);  是否包含元素
set.delete(1);  删除
set.has(1);
```

array

```javascript
let names = ["lucy","jacky","candy","joe","tom"];
function len3(str){
    return str.length>=3;
}
console.log(names.every(len3));//对数组的每一个数据 调用 len3
console.log(names.every(name=>name.length>=3));//对数组的每一个数据 调用 判断 长度是否>=3

console.log(names.filter(name=>name.length===3));//过滤 得到 数组

console.log(names.find(name=>name.length==3));//查找 只返回 第一个de 下标

console.log(Array.from(names,name=>name.concat("^_^")));//数组类的工具方法,从某个数组,经过某个计算,得到新数组

console.log(names.join("~"));//使用连接符把数组里的每个数据连接起来返回字符串

console.log(names.map(name=>name.concat("-_-")));//数组里的每个数据 进行映射 映射为新数组

let stack = ["main","show","say","setColor"];
while(stack.length>0){
    console.log(stack.pop());//弹栈,从数组末尾删除 并得到数据
}
let stack2 = [];
let i = 1;
while(stack2.length<5){
    stack2.push(Math.random()+"="+ i++);//压栈,将数据添加到数组的末尾
}
console.log(stack2);
//total默认是第一个值 name是后面的每一个值,初始化total要放在reduce()中,不能放在log()中
console.log(names.reduce((total,name)=>{return total.concat(name,"=")},"名字大联合:"));
console.log(names.reduce((total,name)=>{return total.concat("=",name)}));

let queue = ["张三","小妹","小梅","黎明"];
console.log("买饭要排队:");
while(queue.length!=0){
    console.log(queue.shift());//从数组开头删除 并 返回数据
}

console.log("部分人员名单:"+names.slice(-2));//从倒数第二个开始 直到最后
console.log("部分人员名单:"+names.slice(0,3));//开始的下标 结束的下标(不包括)

console.log(names.some(name=>name.startsWith("j")));//查找有没有一个数组的数据 符合 开头是j
//let names = ["lucy","jacky","candy","joe","tom"];
//splice 先删除 后添加
/*names.splice(1,1,"小李","桃子")
console.log(names);*///在1下标处删除数据,添加 后面的两个数据,此方法会得到 删除的数据
names.splice(1,0,"小小船");//在1小标出删除0个数据,添加后面1个数据
console.log(names);//['lucy', '小小船', 'jacky', 'candy', 'joe', 'tom']
```

#### 函数

```javascript
 <!--    函数的定义方式一    -->
 function abs(x) {
     if (x > 0) {
         return x;
     } else {
         return -x;
         //执行到retuen代表函数结束，返回结果，
         //如果没有执行return，函数执行完也会返回结果，结果为undefined
     }
 }

 <!--    函数的定义方式二    -->
 //这种形式就是把function (x)当成匿名内部类，但是可以把该类赋值给一个变量，通过变量调用函数
 var abs2 = function (x) {
     if (x > 0) {
         return x;
     } else {
         return -x;
     }
 }

 <!--    规避参数不存在和多个参数问题    -->
 参数不存在时可设置抛出异常  ，typeof可判断变量类型
 var abs3 = function (x) {
     if (typeof x !="number"){
         throw "Not a Number";
     }
     if (x > 0) {
         return x;
     } else {
         return -x;
     }
 }

 对于参数过长
 // es5所用方法，arguments关键子，代表一个数组，会把所有传进来的参数都放到里面
 var abs4 = function (x) {
     console.log(x);
     for (let i = 0; i<=arguments.length; i++){
         console.log(arguments[i]);
     }
 }
 //e6所用方法，rest关键子，剩余的，会把除定义的参数外其他所有传进来的参数都放到里面。...在java代表可变参数
 var abs5 = function (x,y,...rest) {
     console.log(x);
     console.log(y);
     console.log(rest);
 }
```

#### 事件

HTML 事件是发生在 HTML 元素上的事情。当在 HTML 页面中使用 JavaScript 时， JavaScript 可以触发这些事件。HTML 元素中可以添加事件属性，使用 JavaScript 代码来添加 HTML 元素。

```html
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
<button onclick="this.innerHTML=Date()">现在的时间是?</button>
```

| 事件        | 描述                                 |
| :---------- | :----------------------------------- |
| onchange    | HTML 元素改变                        |
| onclick     | 用户点击 HTML 元素                   |
| onmouseover | 鼠标指针移动到指定的元素上时发生     |
| onmouseout  | 用户从一个 HTML 元素上移开鼠标时发生 |
| onkeydown   | 用户按下键盘按键                     |
| onload      | 浏览器已完成页面的加载               |

更多事件：[HTML DOM 事件对象 | 菜鸟教程](https://www.runoob.com/jsref/dom-obj-event.html)

事件可以用于处理表单验证，用户输入，用户行为及浏览器动作:

- 页面加载时触发事件
- 页面关闭时触发事件
- 用户点击按钮执行动作
- 验证用户输入内容的合法性……

#### 表单

HTML 表单验证可以通过 JavaScript 来完成。

以下实例代码用于判断表单字段(fname)值是否存在， 如果不存在，就弹出信息，阻止表单提交：

```javascript
function validateForm() {
    var x = document.forms["myForm"]["fname"].value;
    if (x == null || x == "") {
        alert("需要输入名字。");
        return false;
    }
}
```

```html
<form name="myForm" action="demo_form.php" onsubmit="return validateForm()" method="post">
    名字: <input type="text" name="fname">
    <input type="submit" value="提交">
</form>
```

数据验证用于确保用户输入的数据是有效的。典型的数据验证有：

- 必需字段是否有输入?
- 用户是否输入了合法的数据?
- 在数字字段是否输入了文本?

大多数情况下，数据验证用于确保用户正确输入数据。数据验证可以使用不同方法来定义，并通过多种方式来调用。

**服务端数据验证**是在数据提交到服务器上后再验证。**客户端数据验证**是在数据发送到服务器前，在浏览器上完成验证。

HTML5 新增了 HTML 表单的验证方式：约束验证（constraint validation）。约束验证是表单被提交时浏览器用来实现验证的一种算法。

HTML 约束验证基于：

- **HTML 输入属性**
- **CSS 伪类选择器**
- **DOM 属性和方法**

约束验证 HTML 输入属性

| 属性     | 描述                     |
| :------- | :----------------------- |
| disabled | 规定输入的元素不可用     |
| max      | 规定输入元素的最大值     |
| min      | 规定输入元素的最小值     |
| pattern  | 规定输入元素值的模式     |
| required | 规定输入元素字段是必需的 |
| type     | 规定输入元素的类型       |

约束验证DOM方法：

* checkValidity()：如果 input 元素中的数据是合法的返回 true，否则返回 false。
* setCustomValidity()：设置 input 元素的 validationMessage 属性，用于自定义错误提示信息的方法。

input 元素的 **validity 属性**包含一系列关于 validity 数据属性:

| 属性            | 描述                                                       |
| :-------------- | :--------------------------------------------------------- |
| customError     | 设置为 true, 如果设置了自定义的 validity 信息。            |
| patternMismatch | 设置为 true, 如果元素的值不匹配它的模式属性。              |
| rangeOverflow   | 设置为 true, 如果元素的值大于设置的最大值。                |
| rangeUnderflow  | 设置为 true, 如果元素的值小于它的最小值。                  |
| stepMismatch    | 设置为 true, 如果元素的值不是按照规定的 step 属性设置。    |
| tooLong         | 设置为 true, 如果元素的值超过了 maxLength 属性设置的长度。 |
| typeMismatch    | 设置为 true, 如果元素的值不是预期相匹配的类型。            |
| valueMissing    | 设置为 true，如果元素 (required 属性) 没有值。             |
| valid           | 设置为 true，如果元素的值是合法的。                        |

例如：如果输入的值大于 100，显示一个信息

```javascript
<input id="id1" type="number" max="100">
<button onclick="myFunction()">验证</button>
 
<p id="demo"></p>
 
<script>function myFunction() {
    var txt = "";
    if (document.getElementById("id1").validity.rangeOverflow) {
       txt = "输入的值太大了";
    }
    document.getElementById("demo").innerHTML = txt;
}</script>
```

#### DOM

文档对象，每个网页都是一个DOM树形结构（HTML 文档中的所有内容都是节点），可以对它进行增删改查操作

增删改查：

```javascript
//获取各个节点
document.getElementsByTagName('h1');
document.getElementById('p1');
document.getElementsByClassName('p2');
//获取父节点
document.getElementById('f1');
//获取父节点下所有子节点
f1.children

// 删除dom
//删除父节点下的一个子节点，需要先获取父节点，再删除子节点
var son = document.getElementById('p1');
var father = p1.parentElement;
father.removeChild('p1');

// 更新dom
//首先获取到一个节点
document.getElementById('dom');
//操作文本节点,需要全部使用冒号
dom.innerText='123';
//操作html节点
dom.innerHTML='888nbsp;999'
//操作js节点
dom.style.color='red';
dom.style.fontSize='100px';

// 插入dom
var s1 = document.getElementById('s1');
var s2 = document.getElementById('s2');
//append插入一个已存在的节点
s2.appendChild(s1);
//插入一个新的节点
var s5 = document.createElement('p');//新建一个空的p标签，这个标签叫s5，<p>
s5.id = 's5'; //给s5这个标签设置id为s5
s5.innerText = '这是新建的一个p标签'; //给s5设置内容
s2.appendChild(s5);
```

使用示例：

```javascript
<script type="text/javascript">
   // 1 dom 获取 name属性是hobby 的4个input对象 组合的数组 的长度  把他弹出!
   // 2 修改 .hh2 的h2 标题 的 文本内容为   "我的标题2"
   // 3 将 #zstd 单元格 所在行 文本居中.
   // 4 鼠标悬浮到表格上时  #myjava 被选择!
   //文档加载完毕,才去执行,这样就能获得html内容了
   window.onload=function () {
      alert(document.getElementsByName("hobby").length);
      document.getElementById("hh2").innerText = "我的标题2";//这里也可以写innerHTML(标签,文本)
      //原生的css里是text-align js里变为驼峰了
      document.getElementById("zstd").style.textAlign = "center";
      document.getElementsByTagName("table")[0].onmouseover=function () {
         document.getElementById("myjava").checked="checked";//true 修改浏览器内存里的对象属性 及时的反应在浏览效果上
         //document.getElementById("myjava").setAttribute("checked","checked");//修改源码里的对象的属性
      }
   }
</script>

// 事件
//move是js里 封装的 事件对象,有很多属性可以调用,有阻止默认行为的方法
function momo(){
   //circle跟随鼠标移动而移动!
   //鼠标指针的坐标
   let x = event.clientX;//event为内置对象
   let y = event.clientY;
   document.getElementById("circle").style.left = x - 50+"px";
   document.getElementById("circle").style.top = y - 50+"px";//?????????????????????????
}
//演示 event对象 的 阻止默认行为的 方法
document.getElementById("a1").onclick = function () {
   let b = confirm("确定要打开百度吗?");
   if (b===false){
      event.preventDefault();
   }
}
document.getElementById("province").onchange = function () {
   alert("你选择的省份: "+document.getElementById("province").value);
}
document.getElementById("username").onfocus = function () {
   document.getElementById("username").style.color = "red";
}
document.getElementById("username").onblur = function () {
   document.getElementById("username").style.color = "black";
}
```

#### JQuery

```html
<!-- 导入 -->
<script src="../../lib/jquery-3.7.1.js"></script>
<!-- 使用 -->
<!-- 1.先创建一个标签-->
<a href="" id="jquery">点击事件</a>
<script>
    <!-- 2.通过jquery进行操作   -->
        $('#jquery').click(function (){
        //先选择这个id标签，再操作一个事件，这里操作的是点击事件click，事件里面有个函数
        alert('hello,jquery');
    })
</script>

<ul id="u1">
    <li id="u2">ggz</li>
    <li class="u3">xmm</li>
    <li name="u4">qqz</li>
</ul>

<script>
    // jquery获取值和设置值,都要加''
    $('#u1 li[id=u2]').text('jjz999');
    $('u3').text();
    $('#u1 li[name=u4]').html();
    // 操作css
    $('#u1 li[class=u3]').css({"color":"red"},{"background":"blue"})
    $('#u2').css({"fontSize":"50px"})
    // 文档的显示与隐藏，先设置display: none;，再对某标签进行隐藏和显示
    $('#ul').hide();
    $('#ul').show();
</script>
```

## Vue

Vue是一个构建用户界面UI的**渐进式javascript框架**，渐进式的框架是指可以一步一步的由浅入深的去使用这个框架，该框架可以逐步引入项目。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

Vue2官网（已停止维护）：<https://v2.cn.vuejs.org/>。Vue2教程：[介绍 — Vue.js](https://v2.cn.vuejs.org/v2/guide/)

### 创建Vue实例

1. 准备容器
2. 引包（官网） — 开发版本/生产版本
3. 创建Vue实例  new Vue()
4. 指定配置项，渲染数据
   1. el：指定挂载点，即Vue所管理的容器
   2. data：提供数据，使用插值表达式可以渲染出Vue提供的数据

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
 
<!-- 
  创建Vue实例，初始化渲染
  1. 准备容器 (Vue所管理的范围)
  2. 引包 (开发版本包 / 生产版本包) 官网
  3. 创建实例
  4. 添加配置项 => 完成渲染
-->
 
<!-- 不是Vue管理的范围 -->
<div class="box2">
  box2 -- {{ count }}
</div>
<div class="box">
  box -- {{ msg }}
</div>
-----------------------------------------------------
<!-- Vue所管理的范围 -->
<div id="app">
  <!-- 插值表达式{{data}}，渲染data中的数据 -->
  <h1>{{ msg }}</h1>
  <a href="#">{{ count }}</a>
</div>
 
<!-- 引入的是开发版本包 - 包含完整的注释和警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
 
<script>
  // 一旦引入 VueJS核心包，在全局环境，就有了 Vue 构造函数
  const app = new Vue({
    // 通过 el 配置选择器，指定 Vue 管理的是哪个盒子（这里app对应上面div id=app）
    el: '#app',
    // 通过 data 提供数据
    data: {
      msg: 'Hello 传智播客',
      count: 666
    }
  })
 
</script>
  
</body>
</html>
```

### 响应式特性

Vue 核心特性：响应式：**数据变化，视图自动更新**，数据驱动视图

<img src="Vue/1681888539340.png?lastModify=1733991844" alt="68188853934" style="zoom:67%;" />

### 指令

**概念：**指令（Directives）是 Vue 提供的带有 **v- 前缀** 的 特殊 标签**属性**。Vue 会根据不同的指令，针对标签实现不同的功能

vue 中的指令按照不同的用途可以分为如下 6 大类：

-  内容渲染指令（v-html、v-text）
-  条件渲染指令（v-show、v-if、v-else、v-else-if）
-  事件绑定指令（v-on）
-  属性绑定指令 （v-bind）
-  双向绑定指令（v-model）
-  列表渲染指令（v-for）

#### v-text 内容渲染

内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下2 个：

- v-text（类似innerText）


- - 使用语法：`<p v-text="uname">hello</p>`，意思是将 uame 值渲染到 p 标签中
  - 类似 innerText，使用该语法，会覆盖 p 标签原有内容


- v-html（类似 innerHTML）


- - 使用语法：`<p v-html="intro">hello</p>`，意思是将 intro 值渲染到 p 标签中
  - 类似 innerHTML，使用该语法，会覆盖 p 标签原有内容
  - 类似 innerHTML，使用该语法，能够将HTML标签的样式呈现出来。

代码演示：

```js

<div id="app">
    <h2>个人信息</h2>
// 既然指令是vue提供的特殊的html属性，所以咱们写的时候就当成属性来用即可
<p v-text="uname">姓名：</p> 
<p v-html="intro">简介：</p>
</div> 

<script>
    const app = new Vue({
        el:'#app',
        data:{
            uname:'张三',
            intro:'<h2>这是一个<strong>非常优秀</strong>的boy<h2>'
        }
    })
</script>
```

#### v-if 条件渲染

1. `v-show` 原理是切换 `display:none` 控制元素显示隐藏。适合频繁切换显示隐藏的场景 

   （v-show = "表达式"   表达式值为 true 显示， false 隐藏）

2. `v-if`  基于条件判断，是否创建 或 移除元素节点。要么显示，要么隐藏，适合不频繁切换的场景

```vue
<h3 v-if="age < 18">young</h3>
<h3 v-else-if="age < 40">mid</h3>
<h3 v-else>old</h3>
```

#### v-on 事件绑定

- <button v-on:事件名="内联语句">按钮</button>
- <button v-on:事件名="处理函数">按钮</button>
- <button v-on:事件名="处理函数(实参)">按钮</button>
- `v-on:` 简写为 **@**

双击事件：@dblclick

失去焦点事件：@blur

输入框内容改变、下拉列表改变：@change

```vue
<button v-on:click="count++">按钮</button> 
<button @click="fn">按钮</button>
<button @click="fn(a,b)">按钮</button>
```

v-on配置methods函数

```vue
<body>
  <div id="app">
    <button @click="fn">切换显示隐藏</button>
    <h1 v-show="isShow">黑马程序员</h1>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app4 = new Vue({
      el: '#app',
      data: {
        isShow: true
      },
      methods: {
        fn () {
          // 让提供的所有methods中的函数，this都指向当前实例
          // console.log('执行了fn', app.isShow)
          // console.log(app3 === this)
          this.isShow = !this.isShow
        }
      }
    })
  </script>
</body>
```

参数传递

```vue
<body>
 
  <div id="app">
    <div class="box">
      <h3>小黑自动售货机</h3>
      <button @click="buy(5)">可乐5元</button>
      <button @click="buy(10)">咖啡10元</button>
      <button @click="buy(8)">牛奶8元</button>
    </div>
    <p>银行卡余额：{{ money }}元</p>
  </div>
 
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        money: 100
      },
      methods: {
        buy (price) {
          this.money -= price
        }
      }
    })
  </script>
</body>
```

#### v-bind 属性绑定

**v-bind:**属性名=“表达式”。动态设置html的标签属性 比如：src、url、title

1. **v-bind:**可以简写成 =>   **:**

比如，有一个图片，它的 `src` 属性值，是一个图片地址。这个地址在数据 data 中存储。

则可以这样设置属性值：

- `<img v-bind:src="url" />`
- `<img :src="url" />`   （v-bind可以省略）

##### v-bind增强

为了方便开发者进行样式控制， Vue 扩展了 v-bind 的语法，可以针对 **class 类名** 和 **style 行内样式** 进行控制 。

当class**动态绑定**的是**对象**时，**键就是类名，值就是布尔值**，如果值是**true**，就有这个类，否则没有这个类

```html
<div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div>
```

​    适用场景：一个类名，来回切换

当class动态绑定的是**数组**时 → 数组中所有的类，都会添加到盒子上，本质就是一个 class 列表

```html
<div class="box" :class="[ 类名1, 类名2, 类名3 ]"></div>
```

   使用场景：批量添加或删除类

操作style

```vue
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>
```

#### v-for 列表渲染

v-for 基于数据循环，多次渲染整个元素

v-for 指令需要使用 `(item, index) in arr` 形式的特殊语法，其中：

- item 是数组中的每一项
- index 是每一项的索引，不需要可以省略
- arr 是被遍历的数组

此语法也可以遍历**对象和数字**

```vue
<ul>
  // key作用：给元素添加的唯一标识，便于Vue进行列表项的正确排序复用
  <li v-for="(item,index) in booksList" :key="item.id"> 
    <span>{{ item.name }}</span>
    <span>{{ item.author }}</span>
  </li>
</ul>
```

#### v-model 双向绑定

v-model 用于表单元素。所谓双向绑定就是：

1. 数据改变后，呈现的页面结果会更新
2. 页面结果更新后，数据也会随之而变

**作用：** 给**表单元素**（input、radio、select）使用，双向绑定数据，可以快速 **获取** 或 **设置** 表单元素内容

```html
<body>
  <div id="app">
    账户：<input type="text" v-model="username"> <br><br>
    密码：<input type="password" v-model="password"> <br><br>
    <button @click="login">登录</button>
    <button @click="reset">重置</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        password: ''
      },
      methods: {
        login () {
          console.log(this.username, this.password)
        },
        reset () {
          this.username = ''
          this.password = ''
        }
      }
    })
  </script>
</body>
```

##### v-model增强

常见的表单元素都可以用 `V-model` 绑定关联  →  快速 **获取** 或 **设置** 表单元素的值

会根据控件类型自动选取正确的方法来更新元素

```vue
<input type="checkbox" v-model="isSingle"> 

<input v-model="gender" type="radio" name="gender" value="1">男
<input v-model="gender" type="radio" name="gender" value="2">女

<select v-model="cityId">
	<option value="101">北京</option>
	<option value="102">上海</option>
</select>

const app = new Vue({
  el: '#app',
  data: {
    isSingle: false,
    gender: "1",
    cityId: '101',
  }
})
```

#### 指令修饰符

所谓指令修饰符就是通过“.”指明一些指令**后缀** 不同的**后缀**封装了不同的处理操作  —> 简化代码

`@keyup.enter` 键盘回车enter键监听

`v-model.trim` 去除首尾空格

`v-model.number` 转数字

`@click.stop` 阻止冒泡

#### computed计算属性

基于**现有的数据**，计算出来的**新属性**。 **依赖**的数据变化，**自动**重新计算。

1. 声明在 **computed 配置项**中，一个计算属性对应一个函数
2. 使用起来和普通属性一样使用  {{ 计算属性名}}  

```javascript
computed: {
	计算属性名(){
		基于现有数据，编写求值逻辑
		return 结果
	}
}
```

```vue
<body>
  <div id="app">
    <h3>小黑的礼物清单</h3>
    <table>
      <tr>
        <th>名字</th>
        <th>数量</th>
      </tr>
      <tr v-for="(item, index) in list" :key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.num }}个</td>
      </tr>
    </table>

    <!-- 目标：统计求和，求得礼物总数 -->
    <p>礼物总数：{{ totalCount }} 个</p>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        // 现有的数据
        list: [
          { id: 1, name: '篮球', num: 1 },
          { id: 2, name: '玩具', num: 2 },
          { id: 3, name: '铅笔', num: 5 },
        ]
      },
      computed:{
        //注意是属性不是函数
        totalCount(){
          //0表示求和起始值，reduce遍历list，将每个item的值加上后返回给sum
          let total= this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
    })
  </script>
</body>
```

- **computed计算属性**
  作用：封装了一段对于**数据**的处理，求得一个**结果**
  缓存特性：计算属性会对计算出来的**结果缓存**，再次使用直接读取缓存，依赖项变化了，会**自动**重新计算 -> 并**再次缓存**
- **methods方法：**
  作用：给实例提供一个**方法**，调用以处理**业务逻辑**

#### watch侦听器（监视器）

**监视数据变化**，执行一些业务逻辑或异步操作

```javascript
data: { 
  words: '苹果',
  obj: {
    words: '苹果'
  }
},

watch: {
  // 该方法会在数据变化时，触发执行
  数据属性名 (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  },
  '对象.属性名' (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  }
}
```

示例：

```vue
<body>
    <div id="app">
        <!-- 条件选择框 -->
        <div class="query">
            <span>翻译成的语言：</span>
            <select>
                <option value="italy">意大利</option>
                <option value="english">英语</option>
                <option value="german">德语</option>
            </select>
        </div>

        <!-- 翻译框 -->
        <div class="box">
            <div class="input-wrap">
                <textarea v-model="obj.words"></textarea>
                <span><i>⌨️</i>文档翻译</span>
            </div>
            <div class="output-wrap">
                <div class="transbox">{{ result }}</div>
            </div>
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
        // 接口地址：https://applet-base-api-t.itheima.net/api/translate
        // 请求方式：get
        // 请求参数：
        // （1）words：需要被翻译的文本（必传）
        // （2）lang： 需要被翻译成的语言（可选）默认值-意大利
        // -----------------------------------------------

        const app = new Vue({
            el: '#app',
            data: {
                // words: ''
                obj: {
                    words: ''
                },

                result:'',//翻译结果
                timer: null
            },
            // 具体讲解：(1) watch语法 (2) 具体业务实现
            watch: {
                // 该方法会在数据变化时调用执行
                // newValue新值, oldValue老值（一般不用）
                // words (newValue) {
                //   console.log('变化了', newValue)
                // }

                'obj.words' (newValue) {
                    //console.log('变化了', newValue)
                    //防抖：延迟执行 -> 干啥事先等一等，延迟一会，一段时间内没有再次触发，才执行
                    clearTimeout(this.timer)
                    this.timer = setTimeout(async () => {
                        //这里是ajax的内容
                        const res = await axios({
                            url: 'https://applet-base-api-t.itheima.net/api/translate',
                            params:{
                                words:newValue
                            }
                        })
                        this.result = res.data.data 
                        console.log(res.data.data)
                    }, 300)
                }
            }
        })
    </script>
</body>
```

完整写法
添加额外**配置项**
（1）deep:true 对复杂类型深度监视
（2）immediate:true 初始化立刻执行一次handler方法

```javascript
// 具体讲解：(1) watch语法 (2) 具体业务实现
watch: {
    obj: {
        deep: true, //深度监视
            immediate: true, //立刻执行，一进入页面handler立刻执行
                handler( newValue ){
                //  防抖：延迟执行 -> 干啥事先等一等，延迟一会，一段时间内没有再次触发，才执行
                clearTimeout(this.timer)
                this.timer = setTimeout(async () => {  
                    //这里是ajax的内容
                    const res = await axios({
                        url: 'https://applet-base-api-t.itheima.net/api/translate',
                        params:newValue
                    })
                    this.result = res.data.data 
                    console.log(res.data.data)
                }, 300)
            }
    }
```

### Vue生命周期

1. 创建阶段：创建响应式数据  发送初始化渲染请求
2. 挂载阶段：渲染模版   操作dom
3. 更新阶段：修改数据、更新视图
4. 销毁阶段：销毁实例

<img src="Vue/1682065937815.png?lastModify=1734012561" alt="68206593781" style="zoom:67%;" />

Vue生命周期过程中，会自动运行一些函数（created&mounted），被称为生命周期钩子，让开发者可以在特定阶段运行**自己的代码**

<img src="Vue/1682066040295.png?lastModify=1734057859" alt="68206604029" style="zoom:67%;" />

### 工程化开发

vue开发的两种方式：

- 核心包传统开发模式：基于html / css / js 文件，直接引入核心包，开发 Vue。
- **工程化开发模式：基于构建工具（例如：webpack）的环境中开发Vue。**

Vue CLI 是 Vue 官方提供的一个**全局命令工具**  安装：`npm i @vue/cli -g`
可以帮助我们**快速创建**一个开发Vue项目的**标准化基础架子**【集成 webpack 配置】

<img src="Vue/1682092148521.png?lastModify=1734058353" alt="68209214852" style="zoom:67%;" />

- 组件化：一个页面可以拆分成一个个组件，每个组件有着自己独立的结构、样式、行为
  好处：便于维护，利于复用，有利于提升开发效率
  组件分类：普通组件、根组件
- 根组件：整个应用最上层的组件，包括所有普通小组件

#### App.vue文件

- template：结构（Vue2中有且只能一个元素）
- script：js逻辑
- style:样式（可支持less，需要装包）

```vue
<template>
<!-- Vue2中只能有一个根元素 -->
<div class="app" @click="fn()">
    我是结构
    </div>
</template>

<script>
    export default{
        methods:{
            fn(){
                alert("hello")
            }
        }
    }
</script>


<style lang="less">
    .app {
        width: 400px;
        height: 400px;
        background-color: pink;
    }
</style>
```

#### 组件注册

**局部注册**

- 创建.vue文件（三个组成部分：Header,Main,Footer）
- 在**使用的组件内**（例如app.vue根组件）导入并注册

```vue
<!-- App.vue文件 -->
<script>
import HmHeader from './components/HmHeader.vue'
import HmMain from './components/HmMain.vue'
import HmFooter from './components/HmFooter.vue'

export default {
  components:{
    HmHeader:HmHeader,
    HmMain,
    HmFooter
  }
}
</script>
```

**全局注册**

- 创建.vue文件（三个组成部分）
- **main.js中进行全局注册**

```javascript
//导入需要全局注册的组件
import HmButton from './components/HmButton'

//调用Vue.component进行全局注册
//Vue.component('组件名',组件对象)
Vue.component('HmButton', HmButton)
```

#### 组件组成与通信

写在组件中的样式会 **全局生效** →  因此很容易造成多个组件之间的样式冲突问题。

1. **全局样式**: 默认组件中的样式会作用到全局，任何一个组件中都会受到此样式的影响


2. **局部样式**: 可以给组件加上**scoped** 属性,可以**让样式只作用于当前组件**

```vue
<style lang="less" scoped>
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  min-height: 400px;
}
.el-menu {
  height: 100%;
  border: none;
  h4 {
    color: #fff;
    text-align: center;
    line-height: 10px;
  }
}
.router-link-active {
  text-decoration: none;
  color: #fff;
  font-style: normal;
}
i {
  text-decoration: none;
  color: white;
  font-style: normal;
}
</style>
```

一个组件的 **data** 选项必须**是一个函数**。目的是为了：保证每个组件实例，维护**独立**的一份**数据**对象。

每次创建新的组件实例，都会新**执行一次data 函数**，得到一个新对象。

**组件通信**

组件通信，就是指**组件与组件**之间的**数据传递**

- 组件的数据是独立的，无法直接访问其他组件的数据。
- 想使用其他组件的数据，就需要组件通信

两种组件关系分类 和 对应的组件通信方案

* 父子关系 → props & $emit
  * props：组件上 注册的一些  自定义属性，用于父组件向子组件传递数据
  * 可以 传递 **任意数量/任意类型** 的prop
* 非父子关系 → provide & inject 或 eventbys
* 通用方案 → vuex

##### 父子通信

父->子

<img src="Vue\1682318711785.png?lastModify=1734060490" alt="68231871178" style="zoom: 50%;" />

子->父

<img src="Vue/1682318965635.png?lastModify=1734060558" alt="68231896563" style="zoom: 50%;" />

##### props校验

 为prop指定验证要求，不符合要求，控制台就会有错误提示→帮助开发者，快速发现错误

```javascript
props: {
  校验的属性名: {
    type: 类型,  // Number String Boolean ...
    required: true, // 是否必填
    default: 默认值, // 默认值
    validator (value) {
      // 自定义校验逻辑
      return 是否通过校验
    }
  }
},
```

```javascript
export default {
  // 完整写法（类型、默认值、非空、自定义校验）
  props: {
    w: {
      type: Number,
      //required: true,
      default: 0,
      validator(val) {
        // console.log(val)
        if (val >= 100 || val <= 0) {
          console.error('传入的范围必须是0-100之间')
          return false
        } else {
          return true
        }
      },
    },
  },
}
```

##### props&data

共同点：都可以给组件提供数据
区别：

- data的数据是**自己的** → 随便改
- props的数据是**外部的** → 不能直接改，要遵循单向数据流
  **单向数据流**：父级prop的数据更新，会向下流动，影响子组件。这个数据流动是单向的。

##### 非父子通信

##### event bus 事件总线

1. 创建一个都能访问的事件总线 （空Vue实例）

   ```js
   import Vue from 'vue'
   const Bus = new Vue()
   export default Bus
   ```

2. A组件（接受方），监听Bus的 $on事件

   ```vue
   created () {
     Bus.$on('sendMsg', (msg) => {
       this.msg = msg
     })
   }
   ```

3. B组件（发送方），触发Bus的$emit事件

   ```vue
   Bus.$emit('sendMsg', '这是一个消息')
   ```


##### provide&inject

跨层级数据共享

1. 父组件 provide提供数据

```js
export default {
  provide () {
    return {
       // 普通类型【非响应式】
       color: this.color, 
       // 复杂类型【响应式】
       userInfo: this.userInfo, 
    }
  }
}
```

2.子/孙组件 inject获取数据

```js
export default {
  inject: ['color','userInfo'],
  created () {
    console.log(this.color, this.userInfo)
  }
}
```

- provide提供的简单类型的数据不是响应式的，复杂类型数据是响应式。（推荐提供复杂类型数据）
- 子/孙组件通过inject获取的数据，不能在自身组件内修改

#### 组件双向绑定

表单类组件封装 → 实现子组件和父组件数据的双向绑定
①父传子：数据应该是父组件props传递过来的，v-model拆解绑定数据
②子传父：监听输入，子传父传值给父组件修改
本质：**实现了子组件和父组件数据的双向绑定**

```vue
<!--1.父组件给子组件传递属性cityId        4.父组件监听到'事件名'，更新selectID-->
<BaseSelect :cityId="selectId" @事件名="selectId = $event"></BaseSelect>

<!--3.子组件BaseSelect.vue触发事件handlechange-->
<select :value="cityId" @change="handleChange">...</select>

<script>
    // 2.子组件props接收父组件传值
    props: {
        cityId:String
      },
    methods: {
      // 对应3.触发事件handlechange，给父组件发送消息通知
      handleChange (e) {
        this.$emit('事件名', e.target.value)
       }
     }
</script>
```

##### v-model简化双向绑定

父组件v-model简化代码，实现子组件和父组件数据双向绑定
①子组件中：props通过value接受，事件触发input
②父组件中：v-model给组件直接绑数据

```vue
<!--父组件-->
<BaseSelect v-model="selectId"></BaseSelect>

<!--子组件-->
<select :value="value" @change="handleChange">...</select>

<script>
    //子组件
    props: {
        value:String
    },
        methods: {
            handleChange (e) {
                this.$emit('input', e.target.value)
            }
        }
</script>
```

##### .sync修饰符

作用：可以实现子组件和父组件数据的双向绑定，简化代码
特点：prop属性名，可以自定义，非固定为value
场景：**封装弹框类的基础组件**，visible属性 true显示 false隐藏
本质：就是 :属性名 和 @update:属性名 合写

```vue
<!--父组件-->
<BaseDialog :visible.sync="isShow"></BaseDialog>

<script>
    //子组件
    props:{
        visible:Boolean
      },
      methods:{
         // 关闭弹窗，触发父组件属性隐藏
        close () {
          this.$emit('update:visible', false)
        }
      }
</script>
```

##### ref 和 $refs

作用：利用 ref 和 $refs 可以用于 获取 dom 元素，或 组件实例
特点：查找范围 → 当前组件内（更精确稳定）
① 获取dom：

目标标签 - 添加 ref 属性

```html
<div ref="chartRef">我是渲染图标的容器</div>
```

获取时，通过 this.$refs.xxx,获取目标标签

```vue
<script>
    mounted() {
        console.log(this.$refs.chartRef)
    },
</script>
```

注意：之前只用document.querySelect('.box') 获取的是整个页面中的盒子

##### 异步更新

1. Vue是异步更新DOM的
2. 想要在DOM更新完成之后做某件事，可以使用$nextTick

**语法:** this.$nextTick(函数体)

```js
this.$nextTick(() => {
  this.$refs.inp.focus()
})
```

**注意：**$nextTick 内的函数体 一定是**箭头函数**，这样才能让函数内部的this指向Vue实例

### 单页应用

单页应用程序：SPA【Single Page Application】是指所有的功能都在**一个html页面**上实现

单页应用网站： 网易云音乐  <https://music.163.com/>

多页应用网站：京东  https://jd.com/

<img src="Vue\1682441912977.png" alt="68244191297" style="zoom:67%;" />

单页应用类网站：系统类网站 / 内部网站 / 文档类网站 / 移动端站点

多页应用类网站：公司官网 / 电商类网站 

### 路由

单页面应用程序，之所以开发效率高，性能好，用户体验好

最大的原因就是：**页面按需更新**。比如当点击【发现音乐】和【关注】时，**只是更新下面部分内容**，对于头部是不更新的

<img src="Vue/1682442699775.png?lastModify=1734415679" alt="68244269977" style="zoom: 80%;" />

Vue中的路由：**路径和组件**的**映射**关系

1. 下载 VueRouter 模块到当前工程，版本3.6.5

   ```bash
   yarn add vue-router@3.6.5
   ```

2. main.js中引入VueRouter

   ```vue
   import VueRouter from 'vue-router'
   ```

3. 安装注册

   ```vue
   Vue.use(VueRouter)
   ```

4. 创建路由对象

   ```vue
   const router = new VueRouter({
   	routes:[...]
   })
   ```

5. 注入，将路由对象注入到new Vue实例中，建立关联

   ```vue
   new Vue({
     render: h => h(App),
     router:router
   }).$mount('#app')
   
   ```


当我们配置完以上5步之后 就可以看到浏览器地址栏中的路由 变成了 /#/的形式。表示项目的路由已经被Vue-Router管理了

<img src="Vue\1682479207453-1734415794558.png" alt="68247920745" style="zoom:67%;" />

配置导航，配置路由出口(路径匹配的组件显示的位置)

App.vue

```vue
<div class="footer_wrap">
  <a href="#/find">发现音乐</a>
  <a href="#/my">我的音乐</a>
  <a href="#/friend">朋友</a>
</div>
<div class="top">
  <router-view></router-view>
</div>
```

## 前端框架

经典（以此为准）：[PanJiaChen/vue-element-admin: :tada: A magical vue admin https://panjiachen.github.io/vue-element-admin](https://github.com/PanJiaChen/vue-element-admin)

文档：[介绍 | vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/zh/guide/#功能)

新：[vbenjs/vue-vben-admin: A modern vue admin panel built with Vue3, Shadcn UI, Vite, TypeScript, and Monorepo. It's fast!](https://github.com/vbenjs/vue-vben-admin)

文档：[关于 Vben Admin | Vben Admin](https://doc.vben.pro/guide/introduction/vben.html#页面历史)

### 启动解析

#### 1.main.js入口

示例结构：

```javascript
import Vue from 'vue';        // 引入 Vue
import App from './App';   // 引入根组件
import router from './router'; // 引入路由（如果有）

Vue.config.productionTip = false; // 禁用生产模式下的提示

new Vue({
  el: '#app',   // 将 Vue 实例挂载到 id 为 'app' 的 DOM 元素上
  render: h => h(App),  // 渲染根组件 App
  router,               // 路由配置（如果有）
})
```

#### 2.App.vue

在 Vue 项目中，`App.vue` 是根组件。它通过 `<router-view />` 渲染页面内容。

> Vue Router 是 Vue 官方的客户端路由解决方案。
>
> 客户端路由的作用是在单页应用 (SPA) 中将浏览器的 URL 和用户看到的内容绑定起来。当用户在应用中浏览不同页面时，URL 会随之更新，但页面不需要从服务器重新加载。
>
> Vue Router 基于 Vue 的组件系统构建，你可以通过配置**路由**来告诉 Vue Router **为每个 URL 路径显示哪些组件**。

```vue
<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

#### 3.router

Vue Router 管理应用中的路由和页面导航。路由配置文件`router/index.js`示例：

```javascript
import Vue from 'vue';
import Router from 'vue-router';
import Home from '@/views/Home.vue';
import About from '@/views/About.vue';

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home
    },
    {
      path: '/about',
      name: 'About',
      component: About
    }
  ]
});
```

**实际页面内容的渲染**

1. **加载根组件（`App.vue`）**：
   - 当 Vue 应用启动时，`App.vue` 会被加载并渲染，其中包含了 `<router-view />`。
2. **匹配路由**：
   - 根据 URL 路径，Vue Router 会匹配到相应的路由。
   - 比如，当用户访问 `http://localhost:8080/` 时，Vue Router 会匹配到 `/` 路径，并加载 `Home.vue` 组件。
3. **渲染对应的组件**：
   - Vue Router 会将匹配到的组件（例如 `Home.vue` 或 `About.vue`）插入到 `App.vue` 中的 `<router-view />` 部分。
   - 最终，用户在页面上看到的就是 `Home.vue` 或 `About.vue` 组件的内容。

### 布局

#### Layout

页面整体布局是一个产品最外层的框架结构，往往会包含导航、侧边栏、面包屑以及内容等。 

**vue-router 路由嵌套**：Vue Router 的路由嵌套机制允许在一个路由组件中嵌套显示另一个路由组件。通常在布局（如 `Layout`）中，使用嵌套路由来组织子页面或功能模块。这种嵌套机制通过 `<router-view />` 占位符来渲染子路由组件。

 **一般情况下，你增加或者修改页面只会影响 `app-main`这个主体区域。其它配置在 `layout` 中的内容如：侧边栏或者导航栏都是不会随着你主体页面变化而变化的。**

```text
/foo                                  /bar
+------------------+                  +-----------------+
| layout           |                  | layout          |
| +--------------+ |                  | +-------------+ |
| | foo.vue      | |  +------------>  | | bar.vue     | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

当然你也可以一个项目里面使用多个不同的 `layout`，只要在你想作用的路由父级上引用它就可以了。

**父路由和子路由**：

- **父路由**：是定义在路由配置中，包含子路由的路由。通常，父路由配置会有一个 `<router-view />` 占位符，用于渲染子路由。
- **子路由**：是嵌套在父路由下的路由，子路由通过 `children` 属性来定义。

**路由渲染顺序**：

- 当用户访问某个路径时，Vue Router 会根据路径匹配到父路由，再匹配到子路由。

- 父路由组件渲染时，会渲染一个 `<router-view />` 作为占位符，子路由组件会被插入到这个占位符中。

**1. 路由配置 (`router/index.js`)**

```javascript
javascript复制代码import Vue from 'vue';
import Router from 'vue-router';
import Layout from '@/layouts/Layout';    // 引入 Layout 布局组件
import Dashboard from '@/views/dashboard/index';  // 引入 Dashboard 子页面
import Profile from '@/views/profile/index';      // 引入 Profile 子页面

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: '/',
      component: Layout,  // 父路由使用 Layout 布局组件
      redirect: '/dashboard',  // 默认重定向到 /dashboard
      children: [
        {
          path: 'dashboard',  // 子路由的路径为 'dashboard'
          component: Dashboard,  // 子路由渲染 Dashboard 组件
          name: 'Dashboard',  // 子路由的名称
          meta: { title: 'Dashboard', icon: 'dashboard' }
        },
        {
          path: 'profile',  // 另一个子路由的路径为 'profile'
          component: Profile,  // 子路由渲染 Profile 组件
          name: 'Profile',
          meta: { title: 'Profile', icon: 'user' }
        }
      ]
    }
  ]
});
```

**2. 父路由布局组件 (`Layout.vue`)**

```vue
vue复制代码<template>
  <div class="layout">
    <!-- 侧边栏组件 -->
    <Sidebar />

    <!-- 主体内容区域 -->
    <div class="main-content">
      <!-- 导航栏组件 -->
      <Navbar />

      <!-- 子路由渲染区 -->
      <router-view />  <!-- 这个 <router-view /> 会渲染子路由组件 (如 Dashboard 或 Profile) -->
    </div>
  </div>
</template>

<script>
import Sidebar from '@/components/Sidebar';
import Navbar from '@/components/Navbar';

export default {
  name: 'Layout',
  components: {
    Sidebar,
    Navbar
  }
}
</script>
```

**3. 子路由组件 (`Dashboard.vue` 和 `Profile.vue`)**

**Dashboard.vue**

```vue
vue复制代码<template>
  <div class="dashboard">
    <h1>Dashboard</h1>
    <p>欢迎来到 Dashboard 页面</p>
  </div>
</template>

<script>
export default {
  name: 'Dashboard'
}
</script>
```

**Profile.vue**

```vue
vue复制代码<template>
  <div class="profile">
    <h1>Profile</h1>
    <p>这是用户的个人资料页面</p>
  </div>
</template>

<script>
export default {
  name: 'Profile'
}
</script>
```

##### 实际layout代码

[@/layout](https://github.com/PanJiaChen/vue-element-admin/tree/master/src/layout)  layout下index.vue

```vue
<template>
  <div :class="classObj" class="app-wrapper">
    <div v-if="device==='mobile'&&sidebar.opened" class="drawer-bg" @click="handleClickOutside" />
    <sidebar class="sidebar-container" />
    <div :class="{hasTagsView:needTagsView}" class="main-container">
      <div :class="{'fixed-header':fixedHeader}">
        <navbar />
        <tags-view v-if="needTagsView" />
      </div>
      <!-- 子组件app-main里面定义了子路由router-view  -- 子路由渲染区 -->
      <app-main />
      <right-panel v-if="showSettings">
        <settings />
      </right-panel>
    </div>
  </div>
</template>

<script>
import RightPanel from '@/components/RightPanel'
import { AppMain, Navbar, Settings, Sidebar, TagsView } from './components'
import ResizeMixin from './mixin/ResizeHandler'
import { mapState } from 'vuex'

export default {
  name: 'Layout',
  // 引入并注册子组件，作为layout的一部分进行渲染
  components: {
    AppMain,
    Navbar,
    RightPanel,
    Settings,
    Sidebar,
    TagsView
  },
  // 使用的 mixins，提供了组件间复用的功能
  mixins: [ResizeMixin],
  // 计算属性
  computed: {
    // 使用 mapState 辅助函数映射 Vuex store 中的状态到当前组件的计算属性
    ...mapState({
      // 获取 Vuex store 中的 sidebar 状态，表示侧边栏的开关
      sidebar: state => state.app.sidebar,
      device: state => state.app.device,
      showSettings: state => state.settings.showSettings,
      needTagsView: state => state.settings.tagsView,
      fixedHeader: state => state.settings.fixedHeader
    }),
    // 计算类名对象，返回一个动态的 class 对象
    classObj() {
      return {
        // 根据 sidebar 的状态动态切换类名
        hideSidebar: !this.sidebar.opened, // 侧边栏关闭时，应用 'hideSidebar' 类
        openSidebar: this.sidebar.opened, // 侧边栏打开时，应用 'openSidebar' 类
        withoutAnimation: this.sidebar.withoutAnimation, // 判断是否禁用动画
        mobile: this.device === 'mobile'
      }
    }
  },
  methods: {
    // 处理点击外部区域的事件，用来关闭侧边栏
    handleClickOutside() {
      // 使用 Vuex 的 action 来关闭侧边栏
      this.$store.dispatch('app/closeSideBar', { withoutAnimation: false })
    }
  }
}
</script>
```

* **`<router-view />`**：`Layout` 组件本身并不直接渲染内容，而是作为页面的框架，提供固定的布局部分（如侧边栏、导航栏等）。它通过 `<router-view />` 来渲染子路由的内容。此处没有直接放置 `<router-view />`，而是放置了 `AppMain` 组件，`AppMain` 组件会在其中继续嵌套一个 `<router-view />`，从而实现多层嵌套。

* **`AppMain` 组件**：`Layout` 渲染了 `AppMain` 组件，`AppMain` 内部还通过 `<router-view />` 渲染子路由内容（例如 `Dashboard` 或 `Profile`）。

#### app-main

[@/layout/components/AppMain](https://github.com/PanJiaChen/vue-element-admin/blob/master/src/layout/components/AppMain.vue)

```vue
<template>
  <!-- 主内容区域 -->
  <section class="app-main">
    <!-- 过渡动画：fade-transform -->
    <transition name="fade-transform" mode="out-in">
      <!-- keep-alive 用于缓存视图，以避免重新加载组件 -->
      <keep-alive :include="cachedViews">
        <!-- 渲染路由的子组件，key 用于控制视图的缓存和刷新 -->
        <router-view :key="key" />
      </keep-alive>
    </transition>
  </section>
</template>

<script>
export default {
  name: 'AppMain',
  computed: {
    // 计算属性：获取要缓存的视图列表
    cachedViews() {
      // 从 Vuex 存储中获取缓存视图的列表
      return this.$store.state.tagsView.cachedViews
    },
    // 计算属性：返回当前路由的路径
    key() {
      // 使用当前路由的路径作为唯一的 key 来控制视图缓存和刷新
      return this.$route.path
    }
  }
}
</script>
```

* `transition`：使用 Vue 的过渡系统，为路由切换添加过渡动画。
* `<keep-alive>`标签：Vue 的内置组件，用于缓存页面组件，以避免频繁的销毁和重新创建。
  * `:include="cachedViews"`：指定要缓存的组件列表。`cachedViews` 是一个计算属性，从 Vuex 获取已缓存的视图。
  * 这样配置后，只有在 `cachedViews` 中列出的视图才会被缓存，从而提升性能，避免频繁地销毁和创建页面组件。

* `key`计算属性：返回当前路由的路径。
  * 这个 `key` 属性用于在每次路由切换时更新 `<router-view>`，确保组件在路由变化时重新渲染。
  * 通过使用路由的 `path` 作为 `key`，可以让 Vue 在切换路由时正确地处理组件的缓存与更新。

### Vuex store

**Vuex store** 是 Vue.js 官方提供的一个状态管理库，用于管理应用中的**共享状态**。在 Vue.js 中，状态通常指的是数据（如**用户信息、产品列表、界面设置**等），而这些状态需要在应用的多个组件之间共享和管理。Vuex 通过**集中式的存储和管理这些状态**，使得不同组件之间的数据流变得更加清晰和可维护。

Vuex store 中的状态是**响应式**的，任何使用到这些状态的组件都会在**状态变化时自动更新**。

Vuex store 基于以下几个核心概念：

- **State（状态）**：用于存储应用的共享数据，Vuex 中的状态就类似于一个全局的数据源。
- **Getters（获取器）**：类似于计算属性（computed），用于从 Vuex store 中派生出数据。Getters 用来获取或过滤状态中的数据。
- **Mutations（突变）**：用来同步修改 Vuex store 中的状态。所有对状态的改变必须通过 mutation 来进行。
- **Actions（动作）**：用于处理异步操作和业务逻辑，它们会提交 mutations 来改变状态。
- **Modules（模块）**：Vuex 支持将 store 分割成多个模块，每个模块拥有自己的 state、mutations、actions 和 getters，以便组织大型应用的状态。

```javascript
// store.js
import Vue from 'vue';
import Vuex from 'vuex';

// 安装 Vuex 插件
Vue.use(Vuex);

const store = new Vuex.Store({
  // 状态 (store 中的数据)
  state: {
    counter: 0,
    user: null,
  },

  // 获取器 (计算属性，派生数据)
  getters: {
    counterDouble: (state) => state.counter * 2,
    userName: (state) => state.user ? state.user.name : 'Guest',
  },

  // 突变 (同步修改状态)
  mutations: {
    increment(state) {
      state.counter++;
    },
    setUser(state, user) {
      state.user = user;
    },
  },

  // 动作 (可以进行异步操作)
  actions: {
    fetchUser({ commit }) {
      // 模拟异步获取用户数据
      setTimeout(() => {
        const user = { name: 'John Doe', age: 30 };
        commit('setUser', user);  // 提交 mutation
      }, 1000);
    },
  },

  // 模块 (支持模块化管理状态)
  modules: {
    // 可以定义多个子模块来管理不同的 state
  },
});

export default store;
```

配置store：

```javascript
import Vue from 'vue';
import App from './App.vue';
import store from './store';  // 引入 Vuex store

new Vue({
  render: (h) => h(App),
  store,  // 将 Vuex store 注入到 Vue 实例
}).$mount('#app');
```



## 参考

w3school：[HTML 教程](https://www.w3school.com.cn/html/index.asp)

[前端三件套复习笔记_js前端三件套-CSDN博客](https://blog.csdn.net/qq_58667485/article/details/133761371)

[前端三件套简要笔记_后端转前端 html js css怎么学习-CSDN博客](https://blog.csdn.net/kiddkid/article/details/136520569)

[JavaScript 教程 | 菜鸟教程](https://www.runoob.com/js/js-tutorial.html)

vue：https://www.bilibili.com/video/BV1HV4y1a7n4/?spm_id_from=333.1007.top_right_bar_window_history.content.click

[黑马Vue2+Vue3笔记_vue学习笔记黑马-CSDN博客](https://blog.csdn.net/qq_55666248/article/details/143028853)

[VUE学习笔记（黑马2023版 第四天）_vue 黑马2023版 源码-CSDN博客](https://blog.csdn.net/m0_70675152/article/details/135439211?spm=1001.2014.3001.5502)

[PanJiaChen/vue-element-admin: :tada: A magical vue admin https://panjiachen.github.io/vue-element-admin](https://github.com/PanJiaChen/vue-element-admin)

[介绍 | vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/zh/guide/#功能)