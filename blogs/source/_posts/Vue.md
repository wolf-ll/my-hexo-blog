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



## 参考

w3school：[HTML 教程](https://www.w3school.com.cn/html/index.asp)

[前端三件套复习笔记_js前端三件套-CSDN博客](https://blog.csdn.net/qq_58667485/article/details/133761371)

[前端三件套简要笔记_后端转前端 html js css怎么学习-CSDN博客](https://blog.csdn.net/kiddkid/article/details/136520569)

[JavaScript 教程 | 菜鸟教程](https://www.runoob.com/js/js-tutorial.html)

vue：https://www.bilibili.com/video/BV1HV4y1a7n4/?spm_id_from=333.1007.top_right_bar_window_history.content.click

