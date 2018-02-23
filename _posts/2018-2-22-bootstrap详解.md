---
layout: post
title:  "bootstrap语法详解"
categories: 移动端开发
tags: 移动端开发 bootstrap
excerpt: 本文属于干货类型，详解了bootstrap的各种语法。
---


## 一、基础

#### 1、简介

> bootstrap是来自twitter的前端框架。基于html、css、javascript;移动优先，响应式布局开发。3.0是支持ie8的。由于主要是为了用于移动端，所以没有鼠标移入的那些方法，遇到的时候就只有自己来制作。
>
> 网站：http://getbootstrap.com/;http://github.com/twbs;http://www.bootcss.com/
>
> 使用的时候，需要把bootstrap的css和js文件添加到页面上。
>
> bootstrap的js是基于jq的。

#### 2、栅格系统

> **把整个页面分为12列的网格**，根据不同网格的组合得出布局方案。
>
> 容器：容器分为**流体布局(container-fluid)和固定布局(container)**.通过class来体现出来
```
// 宽度自适应
<div>aaaa</div> 
// 宽度自适应，不过会在左右加上15px的padding
<div class="container-fluid">aa</div>
// 宽度固定依然有padding，不过有@media根据(min-width)来改变宽度
<div class="container">aa</div>

// containner中的宽度设定；根据浏览器宽度，来设置不同的容器宽度
@media (min-width: 1200px)
.container {
    width: 1170px;
}
@media (min-width: 992px)
.container {
    width: 970px;
}
@media (min-width: 768px)
.container {
    width: 750px;
}
// 宽度小于768的时候(明显就是手机的大小)，width：auto;
```
> **列数分配**：根据屏幕宽度的不一样分别使对应的语句生效。当语句不满足当前的屏幕宽度的时候，当前元素就不会管那个class，而使元素的宽度占满整行。**如果一个row里面超出了12列，那么那个div会被排到下一行。**
```

语法：col-lg-num，col-md-num，col-sm-num，col-xs-num;
分别对应宽度：大于1200，大于992，大于768，小于768
num为占用12列中的列数，num>12就不起作用了
row为一行


// 当宽度大于1200px时候
// row下面的col默认是浮动那样的排列，且分别占据1 3 3个格子
// 默认为左浮动pull-left，右浮动为pull-right
<div class="row">
    	<div class="col-lg-1">4</div>
        <div class="col-lg-3">2</div>
        <div class="col-lg-3">3</div>
</div>

// 默认的container是有padding的，即里面的元素不会紧靠着container的边框，但是在他里面加个row，然后就可以解决这个问题
// 即container里面加个row，那么row的那个div就是紧靠着container的边框的，不受padding的影响
<div class="container">
    <div class="row">
    // 下面这个同时用了多个class，即
    // 屏幕宽度大于1200的时候，col-lg优先级最高，每个div是3列
    // 屏幕宽度大于992小于1200的时候，col-md优先级最高，每个div是4列
    // 屏幕宽度大于768小于997的时候，col-sm优先级最高，每个div是6列
        <div class="col-lg-3 col-md-4 col-sm-6"></div>
        <div class="col-lg-3 col-md-4 col-sm-6"></div>
        <div class="col-lg-3 col-md-4 col-sm-6"></div>
        <div class="col-lg-3 col-md-4 col-sm-6"></div>
    </div>
</div>
```
> **列偏移**：向右偏移num个列的距离。**(1)只不过这个只能向右偏移，(2)偏移的num会占用一列总共的num。所以会改变后面元素的位置，把后面的元素往后面挤，(3)如果加上偏移的num超出了总共的12，会使后面的div跳到下一行重新进行偏移**。
```
语法: col-lg-offset-num、col-md-offset-num....
num>12的时候就不起作用了

// 由于第一个div占2列，第二个div占10列，刚好占完一行的12列
// 由于offset会占用总共的num，则第二个div被挤到下一行
    <div class="col-lg-2 col-lg-offset-1"></div>
    <div class="col-lg-10"></div>
    
// 由于push不会占用总共的num，则第一个div有1个列的宽度移动到了第二个div上面，被第二个div覆盖
    <div class="col-lg-2 col-lg-push-1"></div>
    <div class="col-lg-10"></div>
```
> **列排序**：向右或者向左num个列的距离。**(1)这个可以通过push往右移动，也可以通过pull向左移动，(2)由于偏移的num不会占用一列总共的num,所以不会改变后面元素的位置，即只改变自身的位置.(3)当自身的部分移动到后面元素上面的时候，会使自身那部分被后面元素覆盖。**
```
语法: col-lg-push-num、col-lg-pull-num....
push为把元素向右移动，pull为把元素向左移动

// 如果不加列排序class的话，应该是第一个div在左边占2个列，第二个div在右边占10个列
// 这里用了列排序class，把第一个div向右推了10个列，把第二个div向左拉了2个列
// 结果第二个div(占10个列)，排在第一个div(占两个列)前面，
    <div class="col-lg-2 col-lg-push-10"></div>
    <div class="col-lg-10 col-lg-pull-2"></div>
```
> **嵌套**：每一个col里面依然可以嵌套row进去。而嵌套的row的12列，就是相对于包含元素的宽度了。
```
<div class="row">
    <div class="col-lg-4">
       <div class="row">
            <div class="col-lg-4">col-lg-4</div>
            <div class="col-lg-4">col-lg-4</div>
            <div class="col-lg-4">col-lg-4</div>
       </div>
    </div>
    <div class="col-lg-4">col-lg-4</div>
    <div class="col-lg-4">col-lg-4</div>
</div>

```
> **清浮动**：``clearfix``.由于这些col是浮动的方式，所以有时候可能会出现需要清浮动，就用这个。
```
<div class="row">
    <div class="col-lg-8"></div> // 假设这个高度是下面那个div的2倍
    <div class="col-lg-4">col-lg-4</div>
    <div class="clearfix"></div>
    // 如果不用上面那个清浮动，则由于第一个div太高了，
    // 则由于浮动的原理下面这个div不会出现在下一行的开始，而是接着那个高的div后面
    // 所以，这里清浮动之后，下面的div就会出现在开头了
    <div class="col-lg-4">col-lg-4</div>
</div>
```
#### 3、响应式工具

> 概念：针对不同设备显示或者隐藏页面内容。

> **可见类**：当屏幕的宽度在指定范围之间的时候，以指定的方式显示或者隐藏元素
```
语法：-visible-*-*
       >> lg md sm xs
       >> block inline inline-block
      -hidden-*

<div class="row">
    // 当屏幕的宽度在lg的范围内的时候，以block的方式显示元素
    <div class="col-lg-8 visible-lg-block"></div>
    // 当屏幕的宽度在lg或者md的范围内的时候，隐藏元素
    <div class="hidden-lg hidden-md">col-lg-4</div>
</div>
```
> **附加**：**affix**。这个class，使元素就像用来position: fixed;一样。
```
<div class="affix"></div>
```

#### 4、字体图标Glyphicons

> 这个就相当于css3中的@font-face。[bootstrap中的图标库](https://v3.bootcss.com/components/)

> (1)位置：Bootstrap 假定所有的图标字体文件全部位于 ../fonts/(**这个是相对于bootstrap.css文件的**) 目录内.即需要有fonts这个文件夹，且和当前编辑的html文件在同一层目录下，里面装着font文件。
>
> (2)修改位置：由于这个是根据@font-face来的，**路径也是在@font-face中指定**，所以修改也只需要修改那个就好了。如：打开bootstrap.css文件，找到@font-face，然后改就好了。
>
> (3)使用：图标类不能和其它组件直接联合使用。它们不能在同一个元素上与其他类共同存在。应该创建一个嵌套的``` <span>``` 标签，并将图标类应用到这个``` <span>``` 标签上
```
<button type="button" class="btn btn-default">
// glyphicon glyphicon-align-left为图标对应的class
// 当图标纯粹作为装饰用途时需要加上aria-hidden="true"
  <span class="glyphicon glyphicon-align-left" aria-hidden="true"></span>
</button>
```
> (4)自定义：自定义一个字体图标的话。先下载那个图片，然后还是和一起一样使用，不过类名字自定义一个，然后再在css中引入图片和设置宽高那些。
```
.my-icon {background: url(../icon.png) no-repeat;background-size:cover;width:12px;height:12px; }

<span class="glyphicon my-icon" aria-hidden="true"></span>
```

#### 5、预定义样式风格

> 用法：通过把下面的5种预定义样式，加到指定的类上面，来为指定的类提供不同的样式。
```
primary 首选项
success 成功
info 一般信息
warning 警告
danger 危险

// 给按钮加上预定义样式，5种预定义样式为5种颜色
<input type="button" value="按钮">
// class中指定btn为较大的按钮形状，不加的话按钮形状不一样，不过颜色是不会变化的
<input class="btn btn-primary" type="button" value="按钮">
<input class="btn btn-success" type="button" value="按钮">
<input class="btn btn-info" type="button" value="按钮">
<input class="btn btn-warning" type="button" value="按钮">
<input class="btn btn-danger" type="button" value="按钮">

// 给背景加上预定义样式，即给背景加上不同的颜色
<div class="bg-primary">primary</div>
<div class="bg-success">primary</div>
<div class="bg-info">primary</div>
<div class="bg-warning">primary</div>
<div class="bg-danger">primary</div>

// 给文字加上预定义样式，改变文字颜色
<div class="text-primary">primary</div>

// 给警告框加上预定义样式，改变框的颜色
<div class="alert">alert-danger</div>
<div class="alert alert-danger">alert-danger</div>

// 给面板加上预定义样式，制作一个登录框
<div class="panel panel-primary">
  <div class="panel-heading">
    这个是面板的标题
  </div>
  <div class="panel-body">
    <p>这个是面板的主体</p>
    <div class="form-group">
      // 这里这个form-control为一个input的样式
      <label>username: </label><input type="text" class="form-control input-lg">
      <div class="alert alert-warning">用户名错误</div>
      <label>password: </label><input type="password" class="form-control">
    </div>
    <a href="#" class="text-info">忘记密码</a>
    <input type="button" class="btn btn-primary" value="login">
  </div>
</div>
```

## 二、插件

#### 1、按钮

> bootstrap下面要制作按钮的话，首先就要定义基类
```
基类：
    -btn
样式：
    -btn-default(默认)
    -btn-link(链接)
大小：
    -btn-*[lg,sm,xs] （默认为md）（输入框也可以通过-input-*[lg,sm,xs]来指定大小）
    - btn-group-*[lg,sm,xs] (修改整个组的按钮大小)
状态：
    - active (使按钮默认为已经选中的状态，如果再对这个按钮进行选中，则相当于普通按钮的点击状态)
    - disabled（禁用按钮，即鼠标移入，出现禁用的那种）
种类：
    - a,input,button（一般把按钮用在这些上面）
块级：
    - btn-block（把按钮变为块元素，自适应父元素宽度，一般用在移动设备上）
按钮组：
    - btn-group (把里面的按钮，变成按钮组，即按钮连着的地方没有空隙，也没有圆角)
    - btn-group-justified（让按钮自适应为充满父级，对a标签的btn直接生效，button和input需要在外面再包一个btn-group）
    - btn-group-vertical（把按钮变为竖直方向的，不配合btn-group使用）
箭头：
    - caret（形成一个向下的箭头，通过一个div放在button或者a形成的按钮中）（在父元素中加上dropup可以让箭头向上）
   
// 下面这个不是按钮组，是独立的3个按钮
<input class="btn" type="button" value="按钮">
<input class="btn btn-default" type="button" value="按钮">
<input class="btn btn-primary btn-lg active" type="button" value="按钮">

// 下面这个为按钮组，3个按钮连在一起的
<div class="btn-group btn-group-justified">
  <a href="#" class="btn">btn</a>
  <div class="btn-group">
    <input class="btn" type="button" value="按钮">
  </div>
  <div class="btn-group">
    <button class="btn btn-default">按钮</button>
  </div>
</div>

// 竖直排列
<div class="btn-group-vertical">
  <a href="#" class="btn">btn</a>
  <a href="#" class="btn">btn</a>
</div>

// 箭头
<button class="btn dropup">按钮<span class="caret"></span></button>
<div class="btn-group">
  <button class="btn">按钮</button>
  <button class="btn"><span class="caret"></span></button>
</div>
```

#### 2、下拉菜单

> 下拉菜单就在官网上面有实际的例子。

```
* 属性
      - data-* （这个前缀的为js触发器的一种，即操作js的交互的）
      - aria-* （这个前缀为给屏幕阅读器的用户使用的，用来描述当前元素的状态，一般不用）
      - role （这个也是给屏幕阅读器用户使用的，用来描述当前元素是什么，一般不用）
* open （这个在父div上面，即让dropdown-menu是展开状态）
* dropdown-menu-right （加在下拉菜单ul上，让ul靠右边）
* dropdown-header（添加在ul的li上面，让这个li为不能点击的那种）
* divider（添加在ul的li上面，让这个li成为一个分割线）
* text-center （添加在ul的li上面，让li中的文字居中）
 

// 父级需要一个dropdown，即为下拉菜单；如果要使菜单向上展开，则使用dropup
<div class="dropdown">
 // data-toggle="dropdown"中的toggle就像是jq中的那个，点一下显示，再点一下隐藏；dropdown为方法名
 // dropdown-toggle主要是按钮的样式
  <button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown">
    Dropdown
    <span class="caret"></span>
  </button>
  // 菜单需要一个dropdown-menu
  <ul class="dropdown-menu">
    <li class="dropdown-header">第一部分</li>
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li class="divider"></li>
    <li class="dropdown-header">第二部分</li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```

#### 3、选项卡

> 选项卡分为头部和内容两部分。

```
* 头部
      - nav (基础类)
      - nav-tabs （这个类作为选项卡头部的样式）
      - data-toggle="tab" （这个用来切换头部的li，在头部每个li的a标签中加上它）
      - nav-justified （端点对齐，即让子li宽度自适应填满父元素；默认li是里面文字的长度。加在父ul上面）
      - nav-tabs-justified（这个用来调节nav下面的那条线，默认情况下线的长度是父级那么长。加上这个表示自适应li的长度）
      - nav-pills（这个是和nav-tabs不一样的一个样式，其他功能都是一样的）
      - nav-stacked （在nav-pills的条件下使用，让选项卡竖直分布）
* 内容
      - tab-content （这个即定义下面为选项卡的内容）
      - tab-pane （这个需要加在内容的每个li上面，li的数量对应上面头部中li的数量）
      - href对应id（这个用来根据头部li的切换来切换内容。给每个内容li设置id，然后给每个头部li的a的href设置对应的锚点）

<div class="container">
<ul class="nav nav-tabs nav-justified">
   // li里面必须加上a标签，不然选项卡无效
  <li><a href="#a" data-toggle="tab">one</a></li>
  // active为页面加载的时候选中的那个
  <li class="active"><a href="#b" data-toggle="tab">two</a></li>
  // 下面为制作的一个下拉菜单
  <li class="dropdown">
    <a class="dropdown-toggle" data-toggle="dropdown" href="#">three<span class="caret"></span></a>
    <ul class="dropdown-menu">
      <li><a data-toggle="tab" href="#a">11</a></li>
      <li><a data-toggle="tab" href="#c">33</a></li>
    </ul>
  </li>
</ul>
<ul class="tab-content">
// id用上面的href的锚点来对应，tab-pane代表这个是内容
  <li id="a" class="tab-pane">111111</li>
  // active为页面加载的时候选中的那个
  <li id="b" class="tab-pane active">22222</li>
  <li id="c" class="tab-pane">33333</li>
</ul>
</div>
```

#### 4、导航条

> 这个导航条和选项卡实际上是差不多的，只是导航条不需要用对应的锚点切换内容。而且，导航条里面还是可以加上很多按钮：比如按钮，表单，文字等。

```
- 导航条本身
* navbar (添加到父nav元素上面，声明这个是一个navbar)
* navbar-default （添加到父nav元素上面，声明为默认样式浅色调；一般把容器加在nav里面）
* navbar-inverse（添加到父nav上面，为深色调）
* navbar-fixed-top （加在父nav元素上面，让滚动条滚动的时候，导航条一直保持在最上面）
* navbar-fixed-bottom（加在父nav元素上面，让滚动条滚动的时候，导航条一直保持在最下面）
* 遮盖问题（当用了固定定位之后，导航条会覆盖掉一些内容，这个时候就需要给body加上50px的margin值）
- 导航条里面
* nav navbar-nav （添加到ul上，声明导航条，两个都需要）
* navbar-header （这个写在和ul同级的一个div中，是用来包住navbar-brand的a标签的，为了兼容移动端）
* navbar-brand （针对logo的一个类，可以写在a标签中，ul的外面）
* navbar-left （默认，导航li靠左）
* navbar-right （导航li靠左靠右）
* navbar-btn （加在和ul同级的button中，当有按钮的时候，按钮不是垂直居中的，则需要加上这个）
* navbar-link （让链接契合导航条，加载ul外的a上）
* navbar-text （对于文字，让文字居中；和ul同级的p，div这些里面）
* navbar-form （写在和ul同级的form中）
- 移动端隐藏的导航条（即，ul隐藏，然后有一个按钮，点开之后ul再出来）
* 响应式导航条（对pc端是没有效果的）
  - navbar-toggle (写在navbar-header中的button里面)
  - icon-bar（这个用来制作botton中的横线）
  - collapse navbar-collapse（给ul添加一个父级div，放在那上面，就可以让下面的ul隐藏，还需要给这个div指定一个id，用来控制折叠和展开）
  - data-toggle=“collapse” （navbar-toggle中，给按钮指定，触发折叠函数）
  - data-target=“#myExp” （navbar-toggle中，给按钮指定锚点绑定到哪个collapse）
* 滚动监听（即，滚动滚动条，然后菜单滑动到指定的项）
  - data-spy="scroll" (设置在需要监控的元素的父元素上面，比如设在body上)
  - data-target=“#myNav” （即绑定锚点到需要监听的nav上）
  - data-offset=“num” （num为数字；设置在需要监控的元素的父元素上面。即，在距离页面顶部numpx的时候就触发）

<nav class="navbar navbar-inverse navbar-fixed-top">
  <div class="container">
    <div class="navbar-header">
    // 这里data-target锚点指向
      <button class="btn btn-primary navbar-toggle" data-toggle="collapse" data-target="#myExp">
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a href="#" class="navbar-brand">logo</a>
    </div>
    // 这里设置一个id
    <div id="myExp" class="collapse navbar-collapse">
      <ul class="nav navbar-nav">
        <li><a href="#">one</a></li>
        <li class="active"><a href="#">two</a></li>
        <li class="dropdown"><a href="#">two</a></li>
      </ul>
    </div>
    <a href="#" class="navbar-link">this is a link</a>
    <p class="navbar-text">this is text</p>
    <button class="btn btn-primary navbar-btn">btn</button>
    <form class="navbar-form navbar-right">
      <input type="text" class="form-control" name="">
    </form>
  </div>
</nav>
```