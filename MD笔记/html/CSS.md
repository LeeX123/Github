# CSS学习

## CSS Box Model（盒子模型）

----

**所有HTML所有 HTML 元素可以看作盒子，在 CSS 中，"box model "这一术语是用来设计和布局时使用。**

CSS 盒模型本质上是一个盒子，封装周围的 HTML 元素，它包括：边距，边框，填充，和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

下面的图片说明了盒子模型 (Box Model)：

![image-20210725202057544](C:\Users\未来网络\AppData\Roaming\Typora\typora-user-images\image-20210725202057544.png)

不同部分的说明：

- **Margin（外边距）**-  清除边框区域。Margin 没有背景颜色，它是完全透明

- **Border（边框）** - 边框周围的填充和内容。边框是受到盒子的背景颜色影响

- **Padding（内边距）** - 清除内容周围的区域。会受到框中填充的背景颜色影响

- **Padding（内边距）** - 清除内容周围的区域。会受到框中填充的背景颜色影响

## CSS 轮廓（outline）属性

### CSS Outlines

---

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

轮廓（outline）属性指定了样式，颜色和外边框的宽度。

轮廓（outline）属性的位置让它不像边框那样参与到文档流中，因此轮廓出现或消失时不会影响文档流，即不会导致文档的重新显示。

---

### CSS 轮廓（outline）

轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

CSS outline 属性规定元素轮廓的样式、颜色和宽度。

![image-20210725203313823](C:\Users\未来网络\AppData\Roaming\Typora\typora-user-images\image-20210725203313823.png)

## CSS 伪类

---

CSS 伪类是用来添加一些选择器的特殊效果。

由于状态的变化是非静态的，所以元素达到一个特定状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式。由此可以看出，它的功能和 class 有些类似，但它是基于文档之外的抽象，所以叫伪类。

---

### 语法

伪类的语法：

```css
selector:pseudo-class {property:value;}
```

CSS 类也可以使用伪类：

```css
selector.class:pseudo-class {property:value;}
```

### CSS - :lang 伪类

:lang 伪类使你有能力为不同的语言定义特殊的规则

在下面的例子中，:lang 类为属性值为 no 的 q 元素定义引号的类型：

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8"> 
<title>W3Cschool教程(w3cschool.cn)</title> 
<style>
    q:lang(no)
    {
    	quotes: "~" "~";
    }
</style>
</head>

<body>
    <p>一些文字 <q lang="no">段落中的引用</q> 一些文字。</p>
    <p>在这个例子中,:lang定义了q元素的值为lang =“no”</p>
    <p><b>注意:</b> 仅当 !DOCTYPE已经声明时 IE8支持 :lang.</p>
</body>
</html>
```

## CSS 下拉菜单

### CSS 下拉菜单

使用 CSS 创建一个鼠标移动上去后显示下拉菜单的效果。

---

### 基本下拉菜单

当鼠标移动到指定元素上时，会出现下拉菜单。

> 实例
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
> 
>   <title>下拉菜单实例|W3Cschool教程(w3cschool.cn)</title>
>   <meta charset="utf-8">
>   <style>
>   .dropdown {
>       position: relative;
>       display: inline-block;
>   }
> 
>   .dropdown-content {
>       display: none;
>       position: absolute;
>       background-color: #f9f9f9;
>       min-width: 160px;
>       box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
>       padding: 12px 16px;
>   }
> 
>   .dropdown:hover .dropdown-content {
>       display: block;
>   }
>   </style>
> </head>
> <body>
> 
> <h2>鼠标移动后出现下拉菜单</h2>
> <p>将鼠标移动到指定元素上就能看到下拉菜单。</p>
> 
> <div class="dropdown">
>   <span>鼠标移动到我这！</span>
>   <div class="dropdown-content">
>     <p>W3Cschool教程</p>
>     <p>www.w3cschool.cn</p>
>   </div>
> </div>
> 
> </body>
> </html>
> ```
>
> 

### 实例分析

#### HTML 部分

我们可以使用任何的 HTML元素来打开下拉菜单，如：<span>, 或 a <button> 元素。

使用容器元素 (如： <div>) 来创建下拉菜单的内容，并放在任何你想放的位置上。

使用 <div> 元素来包裹这些元素，并使用 CSS 来设置下拉内容的样式。

#### CSS 部分

`.dropdown` 类使用 `position:relative`, 这将设置下拉菜单的内容放置在下拉按钮 (使用 `position:absolute`) 的右下角位置。

`.dropdown-content` 类中是实际的下拉菜单。默认是隐藏的，在鼠标移动到指定元素后会显示。 注意 `min-width` 的值设置为 160px。你可以随意修改它。 **注意:** 如果你想设置下拉内容与下拉按钮的宽度一致，可设置 `width` 为 100% ( `overflow:auto` 设置可以在小尺寸屏幕上滚动)。

我们使用 `box-shadow` 属性让下拉菜单看起来像一个"卡片"。

`:hover` 选择器用于在用户将鼠标移动到下拉按钮上时显示下拉菜单。

### 下拉菜单

创建下拉菜单，并允许用户选取列表中的某一项：

这个实例类似前面的实例，当我们在下拉列表中添加了链接，并设置了样式：

> 实例
> ```html
> <!DOCTYPE html>
> <html>
> <head>
> <title>下拉菜单实例|W3Cschool教程(w3cschool.cn)</title>
> <meta charset="utf-8">
> <style>
> .dropbtn {
>     background-color: #4CAF50;
>     color: white;
>     padding: 16px;
>     font-size: 16px;
>     border: none;
>     cursor: pointer;
> }
> 
> .dropdown {
>     position: relative;
>     display: inline-block;
> }
> 
> .dropdown-content {
>     display: none;
>     position: absolute;
>     background-color: #f9f9f9;
>     min-width: 160px;
>     box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
> }
> 
> .dropdown-content a {
>     color: black;
>     padding: 12px 16px;
>     text-decoration: none;
>     display: block;
> }
> 
> .dropdown-content a:hover {background-color: #f1f1f1}
> 
> .dropdown:hover .dropdown-content {
>     display: block;
> }
> 
> .dropdown:hover .dropbtn {
>     background-color: #3e8e41;
> }
> </style>
> </head>
> <body>
> 
> <h2>下拉菜单</h2>
> <p>鼠标移动到按钮上打开下拉菜单。</p>
> 
> <div class="dropdown">
>   <button class="dropbtn">下拉菜单</button>
>   <div class="dropdown-content">
>     <a href="http://www.w3cschool.cn">W3Cschool教程 1</a>
>     <a href="http://www.w3cschool.cn">W3Cschool教程 2</a>
>     <a href="http://www.w3cschool.cn">W3Cschool教程 3</a>
>   </div>
> </div>
> 
> </body>
> </html>

### CSS 属性 选择器

顾名思义，CSS 属性选择器就是指可以根据元素的属性以及属性值来选择元素。

---

#### 属性选择器

下面的例子是把包含标题（title）的所有元素变为蓝色：

> 实例
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
> <style>
>     [title]
>     {
>     color:blue;
>     }
> </style>
> </head>
> 
> <body>
>     <h2>适用于:</h2>
>     <h1 title="Hello world">你好世界</h1>
>     <a title="w3cschool" href="http://w3cschool.cn">w3cschool</a>
>     <hr>
>     <h2>不适用于:</h2>
>     <p>你好!</p>
> </body>
> </html>
> ```
>
> 

#### 属性和值选择器

下面的实例改变了标题` title='w3cschool' `元素的边框样式:

> 实例
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
> <style>
>     [title=w3cschool]
>     {
>     border:5px solid green;
>     }
> </style>
> </head>
> 
> <body>
>     <h2>适用于:</h2>
>     <img title="w3cschool" src="/statics/images/w3c/logo.png" width="247" height="48" />
>     <br>
>     <a title="w3cschool" href="http://w3cschool.cn">w3cschool</a>
>     <hr>
>     <h2>不适用于:</h2>
>     <p title="greeting">嗨!</p>
>     <a class="w3cschool" href="http://w3cschool.cn">w3cschool</a>
> </body>
> </html>
> ```
>
> 

#### 属性和值的选择器 - 多值

下面是包含指定值的 `title` 属性的元素样式的例子，使用（`~`）分隔属性和值:

> 实例
>
> ```html
> [title~=hello] { color:blue; }
> ```
>
> 

下面是包含指定值的 `lang` 属性的元素样式的例子，使用（`|`）分隔属性和值:

> 实例
>
> ```html
> [lang|=en] { color:blue; }
> ```
>
> 

---

#### 表单样式

属性选择器样式无需使用 class 或 id 的形式:

> 实例
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
> <style>
>     input[type="text"]
>     {
>     width:150px;
>     display:block;
>     margin-bottom:10px;
>     background-color:yellow;
>     }
>     input[type="button"]
>     {
>     width:120px;
>     margin-left:35px;
>     display:block;
>     }
> </style>
> </head>
> <body>
> 
>     <form name="input" action="demo-form" method="get">
>         
>         Firstname:<input type="text" name="fname" value="Peter" size="20">
>         Lastname:<input type="text" name="lname" value="Griffin" size="20">
>         <input type="button" value="Example Button">
>     
>     </form>
> </body>
> </html>
> ```
>
> 

