---
layout: post
title:  "用zen coding来快速编写html"
categories: html
tags: html zen-coding 
excerpt: zen coding是一种快速编写html的方法，这里简要介绍一下这个方法
---


---
**注：zen coding为编写html的一种方法。若，在sublime中用这种方法的话需要安装emmet插件**

## 一、zen coding使用方法

>+ Zen Coding由两个核心组件组成：一个缩写扩展器(缩写为像CSS一样的选择器)和上下文无关的HTML标签对匹配器.
>
>+ 直接在编辑器中输入HTML或CSS的代码的缩写，然后按tab键就可以拓展为完整的代码片段。
 
## 二、语法

 (1)**选择器**:
 
 >+ 后代：>,兄弟：+，上级：^. 如：
 
    缩写：div+div>p>span+em^bq
     <div></div>
     <div>
     <p><span></span><em></em></p>
     <blockquote></blockquote>
     </div>
     
  (2)**分组**：就为一个括号()，表示里面的东西是一个整体。
  
  (3)**乘法**：就用 \* 表示。就是一个普通乘法，div*5就表示有5个div。
  
  (4)**自增符号**：就用$表示。想显示几位数就使用几个$,$@-倒序，$@3从3开始计数。
  
    缩写：ul>li.item$$$*2
    <ul>
    <li class="item001"></li>
    <li class="item002"></li>
    </ul>
    缩写：ul>li.item$*2
    <ul>
    <li class="item1"></li>
    <li class="item2"></li>
    </ul>
    
  (5)**id、类、属性**: 和css一样id用#，class用 . ，属性用[]里面加东西
  
    缩写：form#search.wide
    <form id="search" class="wide"></form>
    缩写：div[a='value1' b="value2"]
    <div a="value1" b="value2"></div>
    
  (6)**文本**：用{}表示，里面加文本。
  
     缩写：a{Click me}
     <a href="">Click me</a>

  (7)**隐式标签和缩写**：
  
  >+ 隐式标签 就是符合html逻辑的情况下，会自动补上合适的标签，比如 
  
    缩写: .class。<div class="class"></div>
    
  >+ 所有未知的缩写都会转换成标签
  
    缩写：a。<a href=""></a>
    缩写：img。<img src="" alt="">
    缩写：input<input type="text">
    别名：input:p。<input type="password" name="" id="">
    缩写：input:s。<input type="submit" value="">
    别名：input:h。<input type="hidden" name="">