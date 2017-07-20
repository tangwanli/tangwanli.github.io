---
layout: post
title:  "Jquery事件和方法"
date:   2017-7-20 17:07:00
categories: JQuery
tags: JQuery JavaScript
---

* content
{:toc}

### 一、jQuery中的事件

  ##### 一、事件绑定
  
  (1)**bind()方法**：用bind()方法对匹配元素进行特定的事件绑定。
  
    语法：bind(type,[data],fn)
    第一个参数为事件类型；
    第二个参数为可选参数，作为event.data属性值传递给事件对象的额外数据对象；
    第三个参数是用来绑定的处理函数
    $(function(){
        $("#id").blind("mouseover",function(){
            $(this).next().show();
        })
    })
    
   (2)**简写绑定事件**：像上面那种还可以简写，如下
   
    $("#id").mouseover(function(){ // 即减少代码量
        $(this).next().show();
    })
    $("#id").click(function(){
        $(this).next().show();
    })
    
 #####  二、合成事件：jQuery有两个合成事件---hover()方法和toggle()方法。类似于前面的ready()方法，都是jq自定义方法
 
  (1)hover()方法：用于模拟光标悬停事件，当光标移动到元素上面时，会触发第一个函数，光标移出时，触发第二个函数。
  
    $(function(){
        $("#id").hover(function(){ // 这个方法替换的是mouseenter和mouseleave
            $(this).next().show();
        },function(){
            $(this).next().hide();
        });
    })
    
   (2)toggle()方法。
   
 ##### 三、事件冒泡　　propagation: 传播
 
 (1)**什么是事件冒泡**：假设页面有2个元素，ul li。li嵌套在ul中，并且都绑定了click事件，同时<body>上面耶绑定了click事件。那么当触发内部<li>的click事件的时候，外部的两个click都会被触发。
 
 (2)**事件对象**：使用事件对象，**只需为函数添加一个参数**。如下代码，只需要**点击<li>元素，事件对象就被创建了**。而，事件对象只有事件处理函数才可以访问到。事件处理函数执行完毕之后，事件对象就会被摧毁。
 
    $("li").click(function(event){ // 这里加了一个event参数
        event.stopPropagation(); // 停止事件冒泡
    })
    
  (3)**阻止默认行为**：用**preventDefault()方法**,可以阻止元素的默认行为。而，如果想要**同时停止事件冒泡和阻止默认行为**，可以**直接在事件处理函数中返回false**。如：
  
    $("a").mouseover(function(){ 
        return false; // 停止事件冒泡和组织默认行为
    })
    
##### 四、事件对象的属性:jQuery对事件对象的常用属性进行了封装。

  (1)**event.type**: 该方法可以**获取事件的类型**
  
    $("a").click(function(event){
        alert(event.type); // 运行后输出   "click"
        return false; // 防止链接跳转
    })
     
  (2)**event.preventDefault()方法** ： 阻止默认行为。
  
  (3)**event.stopPropagation()方法**：停止事件冒泡。
  
  (4)**event.target**: **获取触发事件的元素**。
  
    $("a['href="http://goole.com']").click(function(event){
        alert(event.target.href); // 运行后输出   "href="http://goole.com"
        return false;
    })
    
   (5)**event.pageX和event.pageY**: **获取光标相对于页面的x和y坐标**
   
    $("a").click(function(event){
    // 获取鼠标当前相对于页面的坐标
        alert("current mouse position" + event.pageX + + "," +event.pageY);
        return false; 
    })
    
   (6)**event.which和event.keyCode**: 该属性用于确定你按下的是哪一个键盘按键或鼠标按钮。**which属性的返回值是Number类型，返回触发当前事件时按下的键盘按键或鼠标按钮。**
   
   >+ 在mousedown、mouseup事件中，event.which属性返回的是对应鼠标按钮的映射代码值(相当于event.button)。以下是主要的鼠标按钮映射代码对应表
   ![](f:/markdown专用/which1.png)
   >
   >+ 在keypress事件中，event.which属性返回的是输入的字符的Unicode值(相当于event.charCode)。以下是常用的字符Unicode代码对应表。
   ![](f:/markdown专用/which2.png)
   >
   >+ 在keydown、keyup事件中，event.which属性返回的是对应按键的映射代码值(相当于event.keyCode)。以下是常用的键盘按键映射代码的对应表：
   ![f:/markdown专用/which3.png]()
   
    $("input").keydown(function(event){ // 当键盘按下键时候触发的事件
        alert(event.which); 
    })
    $("a").mousedown(function(event){ // 当键盘按下键时候触发的事件
        alert(event.which);  // 1=鼠标左键，2=鼠标中键，3=鼠标右键
    })
    
##### 五、移除事件

 (1)**unbind()方法**：用于移除事件。
 
    $('#delAll').click(function(){
        $('#btn').unbind(); // 解除#btn原本带有的事件
    })
    
(2)**one()方法**：为元素绑定处理函数，但是，当处理函数触发一次之后，立即被删除。

    语法：one(type,[data],fn)// 和bind()方法语法相似
    $(function(){
        $('#btn').one("click",function(){
            $('#test').append("<p>what</p>");
        });
    })
    
##### 六、模拟操作：平常用户必须通过单机按钮才能触发(click)事件，用trigge()方法可以通过模拟用户操作，来达到单击的效果。比如，在进入页面之后，直接触发(click)事件，而不需要用户主动单击。

   (1)**trigger()方法**：trigger:触发器。语法：trigger(event,[data]).**第一个参数是要触发的事件类型**，**第二个参数是要传递给事件处理函数的附加数据，以数组形式传递**。
   
   >+ **常用模拟**：$('#btn').trigger("click"); 也可以简写为 $('#btn').click();
   >
   >+ **触发自定义事件**：
   
       例如为元素绑定了一个"myClick"事件
       $('#btn').bind("myClick",function(){
           $('#test').append("<p>what</p>");
       })
       然后用trigger()方法来进行触发
       $('#btn').trigger("myClick");
       
       下面是另外一个自定义事件执行
       $("input").select(function(){
       $("input").after("文本被选中！");
       });
       var e = jQuery.Event("select");
       $("button").click(function(){
       $("input").trigger(e);
      });
>
>+ **执行默认操作**：trigger()方法触发事件之后，还会执行浏览器的默认操作。如果想要只触发绑定的事件，不执行默认操作，那么需要用**triggerHandler()**方法。

---
   ### 二、jQuery中的动画
   
   ##### 一、show()和hide()、fadeIn()和fadeOut()
   
   (1)**show()和hide()**：这两个方法同时改变元素的**宽、高、不透明度**三个属性。用**hide**()方法时候，相当于是把元素设置为**display：none**。然后调用**show**()方法，再把元素的display设置为以前的那个值。
   
   (2)**fadeIn()和fadeOut()**：这两个方法只改变元素的不透明度。fadeIn()为增加不透明度，fadeOut()为降低元素的不透明度。fadein：渐明，渐显。fadeout：渐隐，渐没。
   
   (3)**slideUp()和slideDown()**: 这两个方法只改变元素的高度。slideUp()为通过使用滑动效果，从下到上缩短隐藏被选元素。slideDown()为从上到下延伸显示被选元素。slide：滑动。
   
   (4)**设置速度参数**：jQuery中的任何动画效果都可以指定3种速度参数(**"slow"、"normal"、"fast"**),时间长度分别代表(**0.6s,0.4s,0.2s**)。当使用这种**关键字的时候需要加上引号**，如：**show("slow")**。还可以直接用**数字作为时间参数**，就**不需要加引号了**，如：**show(1000)**; 另：show()里面的参数**1000为1000毫秒，即1秒钟。**
   
   ##### 二、自定义动画方法animate()
   
   (1)**语法**：animate(style,speed,callback)。第一个参数为**要设置的样式**，即需要执行的动画是什么样子。第二个参数是动画的**速度参数**。第三个参数是在**动画完成时执行的函数**。
   
   (2)**同时执行多个动画**：
   
     $(this).animate({left: "+=500px",opacity: "1"}, 400) // 在0.4s的时间内，将left加上500px，并且把opacity设置为1
     
   (3)"按顺序执行动画"：**css()方法不会加入到动画队列，而是立即执行，所以一般把它放到回调函数里面**。
   
      $(this).animate({left: "+=500px"}, 600).animate({height:"-=10px",width:"+=20px"},200,function(){
          $(this).css("background","red").css({"height":"500px","width":"200px"});
      });// 先执行第一个animate，再执行第二个。第二个animate先运行0.2s，然后进入回调函数，执行里面的语句，改变css样式。
      
  ##### 三、停止动画和判断是否处于动画状态
  
  (1)**stop()方法**: **设置stop(true,true)用于防止 动画积累**。

  >+ 语法：stop([clearQueue],[gotoEnd]);这两个参数都是Booleam值。
  >
  >+ 无参数：立即停止当前正在运行的动画，如果接下来还有动画，则以当前状态开始接下来的动画。如下：**如果鼠标移入之后很快就移出，但是此时还在执行第一个animate**，那么他不会马上执行第二段动画，而是，只停止第一个animate，在运行第二个animate。
  >
  >+ 第一个参数： 代表是否要清空未执行完的动画队列。设置这个参数为true，即stop(true),那么将要直接执行第二段动画，清空第二个animate。
  >
  >+ 第二个参数： 代表是否直接将正在执行的动画跳转到末状态。设置这个参数为true，即**stop(true，true)**,那么将首先停止当前状态，并且到达当前动画末状态，并且清空队列。
  
    $("#panel").hover(function(){
        $(this).stop() // 第一段动画
                   .animate({left: "+=500px"}, 600) // 第1个animate
                   .animate({height:"-=10px",width:"+=20px"},200); // 第2
      }), function(){
          $(this).stop() // 第二段动画
                   .animate({left: "+=500px"}, 600) // 第3
                   .animate({height:"-=10px",width:"+=20px"},200); // 第4
      }
    });
      
  (2)**判断是否处于动画状态**：
  
     if($('#what').is(":animate")){}  // 直接用is()方法判断
     
  (3)**延迟动画**：**delay()方法**，对队列中的下一项的执行设置延迟。
  
     $(this).animate({left: "+=500px"}, 600)
             .delay(1000)
             .animate({left: "+=500px"}, 600) // 执行完第一个动画之后，延迟1s钟，再执行第二个动画
      
 ##### 四、其他动画方法
 
   (1)toggle()方法：用于切换元素的可见状态。
   
      第一个用法：
         $(this).next().toggle(); // 如果是隐藏则切换为可见，如果是可见则切换为隐藏
      第二个用法：
        $("p").toggle(function(){
       $("body").css("background-color","green");},
       function(){
       $("body").css("background-color","red");},
       function(){
       $("body").css("background-color","yellow");
       });

   (2)slideToggle()方法：通过高度，切换元素可见性。
   
   (3)fadeToggle()方法：通过透明度，切换元素可见性。
   
   (4)**fadeTo()方法**：**把元素的不透明度以渐进的方式调整到指定的值**。
   
      语法：fadeTo(speed,opacity,callback); // 第一个参数为速度，第二个参数为调整到多少清晰度，第三个参数为回调函数。
      $(this).next().fadeTo(600,0.2); // 0.6秒内把清晰度变为0.2
     
   
   
   