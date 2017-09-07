---
layout: post
title:  "响应式web设计HTML5和CSS3实战笔记"
categories: html
tags: html5 css3 html
excerpt: 由于这本书的内容很多，所以很多章节的笔记在其他的文章里面
---

## 第一章、响应式web设计基础

 > **响应式web设计**：就是网页内容会随着访问它的视口以及设备的不同而呈现不同的样式。非常实际的做法是：从最基本的体验开始，逐步扩充（即，**渐进增强**、）。而，先做出大而全的版本，然后针对能力不足的平台寻找后背方案（平稳退化）则麻烦的多。
 
### 1.41、html修改
 + **视口**：浏览器中用于呈现网页的区域。视口通常并不等于屏幕大小，特别是可以缩放浏览器窗口的情况下。
 + 需要在<head>中加上一下这行代码,使浏览器按照设备的宽度来渲染网页内容
 ```
 <meta name="viewport" content="width=device-width">
 ```
 
### 1.42、图片

> 在不设置样式的情况下，如果一个图片过大则会让整个页面看起来都失衡。
+ 在css文件中加入下面这行代码可以解决这个问题。但是，一般来说应该先设置一些默认值，这些稍后讨论。
```
img {
    max-width: 100%;
}
```
**注：max-width使图片最大显示为其自身的100%。若，包含图片的元素比图片的固有宽度小，图片会被缩放。**
+ **为什么不用width：100%**：max-width保证了图片的最大值，而如果给width设置了一个值，图片就会按照那个值显示，而不考虑自身的固有宽度。设置为width：100%；那么图片的宽度永远会等于容器宽度。

### 1.43、媒体查询

> 媒体查询可以让我们在某些条件下（如，宽度和高度为多少的情况下）来为网页应用样式。
+ **rem单位**：**这个是css3新引入的单位。em是相对于父元素设置，而rem是相对于根元素<html>设置**。如：

```
html {font-size: 62.5%;/*10 ÷ 16 × 100% = 62.5%*/}
body {font-size: 1.4rem;/*1.4 × 10px = 14px */}
h1 { font-size: 2.4rem;/*2.4 × 10px = 24px*/}
```
**注：这就意味着可以像使用px一样，很方便的使用rem这个单位。浏览器支持有：Firefox 3.6+、Apple Safari 5+、Google Chrome、IE9+和Opera11+。ie8以下就憋屈了。**
+ 下面来关注一种媒体查询，即**最小宽度媒体查询**，**@media**告诉浏览器这里是一个媒体查询；**screen**（从技术上讲，不需要再这里声明“屏幕”）告诉浏览器这个规则适用于屏幕类型；而**and（min-width：50em**）的意思是其中的规则只适合视口宽度在50em以上的情况

```
@media screen and (min-width: 50em) {
    /* 样式 */
}
```
**注：如果在媒体查询的外面写一条规则，实际上是针对所有媒体的“基本”样式。在此基础上，可以再针对不同能力的设备加以拓展。**


## 第二章、媒体查询

### 2.1、为什么响应式web设计需要媒体查询

> css3媒体查询可以让我们针对特定的设备能力或者条件为网页应用特定的css样式。媒体查询就相当于css中的条件处理。
> 除了IE8以下，几乎所有的浏览器都支持它。

### 2.2 媒体查询的语法

+ css文件中使用媒体查询（若宽度为1000px，则会用下面的那条媒体查询，原理是：在目标相同的情况下，下面的css样式会覆盖掉上面的css样式）
```
@media screen and (min-width: 320px) {
    body {
        background: red;
    }
}
@media screen and (min-width: 960px) {
    body {
        background: green;
    }
}
```
**注：screen为默认值，可以省略它和它之后的and。**
+ link中使用媒体查询。即，表示在符合media中的条件的时候，用后面的样式表。orientation: 定位。 portrait：肖像，垂直。
```
<link rel="stylesheet" media="screen" href="#">
<link rel="stylesheet" media="screen and (orientation: portrait)" href="#"> // 表示询问设备是否为有屏幕，且为垂直朝向
<link rel="stylesheet" media="not screen and (orientation: portrait)" href="#">
```
**注：在媒体查询表达式开头加一个not就可以把条件反过来**

### 2.3、组合媒体查询

> 可以组合多个媒体查询，而且只要有一个条件为真就会用样式，且用逗号分隔每个媒体查询表达式。projection: 投影，预测
 ```
<link rel="stylesheet" media="screen and (orientation: portrait) and (min-width: 800px), projection" href="#">
```
+ 下面列出媒体查询3级的特性，用的最多的还是**width**，偶尔会用到分辨率和视口高度。
    + width: 视口宽度
    + height：视口高度
    + device-width：渲染表面的宽度，即设备屏幕的宽度（在媒体查询4级中被废除）
    + device-height：渲染表面的高度，即设备屏幕的高度（在媒体查询4级中被废除）
    + orintation：设备方向是水平还是垂直
    + aspect-ratio：视口的宽高比。如：16/9
    + color：颜色组分的位深。比如，min-color：16表示设备至少支持16位色
    + color-index：设备颜色查找表中的条目数，必须是数值，且不能为负
    + monochrome：单色帧缓冲中表示每个像素的位数，值必须是整数，且不能为负。monochrome：单色的，黑白的
    + resolution：屏幕或者打印分辨率。如：min-resolution：300dpi
    + scan：针对电视的逐行扫描（progressive）和隔行扫描（interlace）。如：72p HD TV（p为progressive）
    + grid：设备基于栅格还是位图
    
**注：上述特性，除了scan和grid之外，后可以加上min或者max前缀以指定范围**

### 2.5、组织和编写媒体查询的注意事项

> **一般都把媒体查询写在样式表中，且将它们分别写在相关的选择符下面。这样，容易维护。若，符合条件，媒体查询中的类会直接覆盖掉原有类**
```
.left {
    width: 20%;
    height: 50px;
    font-size: .5rem;
}
@media (min-width: 40rem) {
    .left {
        width: 30%;
    }
}
```
**注：在没有媒体查询介入的情况下，width为20%，height为50px；随着屏幕变大，width变成了30%，但是height还是为50px，因为不需要修改他们**

### 2.7、关于视口的meta标签

> 这个用于视口的meta标签是网页与移动浏览器的借口。网页通过这个标签告诉浏览器，他希望浏览器如何渲染当前页面。
```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="viewport" content="width=device-width, maximum-scale=3.0,minimum-scale=0.5">
<meta name="viewport" content="width=device-width, user-scalable=no">
```
+ 第一个meta表示：视口宽度等于设备宽度，且把内容放大为实际的1倍
+ 第二个meta表示：允许用户最大将页面放大到设备宽度的3倍，最小缩小至设备宽度的一半
+ 第三个meta表示：禁止用户缩放


## 第三章、弹性布局与响应式图片

> + 2015年css推出了一个新的布局模块叫**弹性盒子（Flexbox）**
> + 除了用来实现弹性布局，Flexbox还可以用来居中内容，改变标记的源码顺序，创建惊艳的页面布局。

### 3.1、将固定像素大小转换为弹性比例大小

> **结果 = 目标/上下文。即，用元素大小除以元素所在容器大小，就是元素占的比例**。如：
```
有一个布局宽度为900px；左边为200px，则左边为的width：20.8333333%。
```
**注：通常来说比例制后面小数的位数不省略比较好，这样使结果更加精确。**

### 3.2、Flexbox概述

> 见另一篇文章

### 3.4、响应式图片

> srcset属性：用来指向提供的图片资源，可以根据屏幕密度现实对应尺寸图片。
>
> sizes属性：用来表示尺寸临界点，主要跟响应式布局打交道。
```
<img class="image" src="mm-width-128px.jpg"
     srcset="mm-width-128px.jpg 128w, mm-width-256px.jpg 256w, mm-width-512px.jpg 512w"
     sizes="(max-width: 360px) 340px, 128px">
```
> 分析：size = "(max-width: 360px) 340px, 128px"表示当视区宽度不大于360像素时候，图片的宽度限制为340像素，其他情况下，使用128px。**w**用来描述文件的宽度，我们可以形象理解为规格。这里的256w并不是指图片的宽度，而是图片的宽度规格，例如，一张图片实际宽度是256像素，但是，这种图片是png24无损图片，或100%质量JPG图片，则，我们可以使用512w表示这张图片，质量好规格就高。

> picture元素：可以用picture元素为不同的视口提供不同的图片。
```
<picture>
<source media="(min-width:30em)" srcset="cake.jpg">
<source media="(min-width:60em)" srcset="car.jpg">
<img src="water.jpg">
</picture>
```
> 分析：首先<picture>元素只是一个容器，source标签中告诉浏览器什么媒体查询之后用什么图片。

## 第四章、HTML5与响应式Web设计

> 查看h5语义化元素

## 第五章、css3新特性

> 查看另外一篇文章

## 第六章、css3高级技术

> 查看另一篇文章

## 第七章、SVG与响应式Web设计

> SVG: 可伸缩矢量图。可以用用Adobe Illustrator设计制作。优点;
```
文件体积小，能够被大量的压缩
图片可无限放大而不失真(矢量图的基本特征)
在视网膜显示屏上效果极佳
能够实现互动和滤镜效果
```

### 7.1、SVG基础

>+ SVG根元素：它的根元素有width、height、和viewbox。视框(viewbox)定义了SVG中所有形状都要遵循的坐标系。```viewBox="0 0 198 198"```前面两个值可以表示左上角，后两个值表示右下角。
```
<svg width="198px" height="188px" viewBox="0 0 198 198">
<svg width="198px" height="188px" viewBox="0 0 99 99"> // 对于这个，里面的形状为了填满SVG的宽高，就会被放大
```
>+ 标题和描述标签：title和desc标签，相当于alt，可以在图像不可见的情况下描述图像内容。
```
<title>star</title>
<desc>create</desc>
```
>+ defs标签：这个标签是用于储存所有可以复用的元素定义的地方，比如梯度、符号、路径。
>+ 元素g：g元素能把其他元素捆绑在一起，如需要画一辆车，就会把构成车轮的形状用g标签集合起来。
```
<g id="page" stroke="none" stroke-width="1" fill="none">
```
>+ SVG形状：SVG有一些些现成可以用的形状（path、rect、circle、ellipse、line、polyline、polygon）

### 7.2、在web页面插入SVG

>+ 使用img标签：直接放在img里面，这种情况下SVG能做的和其他插入的图片差不多。
>+ 作为背景图片插入：插到background里面去，这种方式没有什么特别之处。不过可以用data URI引入。这个方法可以节省一次网络请求。
>+ 使用object标签：object是w3c推荐的用于装载非html内容的容器。data中为svg资源，type为mime类型。通过这种方式添加的svg可以被js访问。
```
<object data="img/svgfile.svg" type="image/svg+xml">
  <span>this</span>
</object>
```
>+ 内联SVG：可以直接把SVG插入到HTML中。

## 第八章、css3过渡、变形和动画

> 查看另一篇文章

## 第九章、表单

> 见另一篇文章。