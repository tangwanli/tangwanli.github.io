---
layout: post
title:  "ajax设置http首部"
categories: JavaScript
tags: JavaScript
excerpt: 通过JQuery中的ajax来设置http请求中的首部。
---


+ jQuery Ajax可以通过headers或beforeSend修改http请求首部，例如： 
```
$.ajax({  
    url: "./test.php",  
    type: "POST",  
    headers: {  
        "Accept" : "text/plain; charset=utf-8",  
        "Content-Type": "text/plain; charset=utf-8"  
    },  
    /* 
    beforeSend: function(jqXHR, settings) { 
        jqXHR.setRequestHeader('Accept', 'text/plain; charset=utf-8'); 
        jqXHR.setRequestHeader('Content-Type', 'text/plain; charset=utf-8'); 
    }, 
    */  
    data: {"user" : "min", "pass" : "he"},  
    error: function(jqXHR, textStatus, errorThrown) {  
        //....  
    },  
    success: function(data, textStatus, jqXHR) {  
        //....  
    }  
}
```
> 注意：：W3规定XMLHttpRequest并不能修改全部的HTTP Headers，而仅是一小部分。 
```
比如： 
Host: localhost:58188 
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:31.0) Gecko/20100101 Firefox/31.0 
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 
Accept-Language: zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3 
Accept-Encoding: gzip, deflate 
Connection: keep-alive 
role:2 
username:106110454@qq.com 
password:lw=kQH 
password:1 
```
+ jquery获取HTTP 的响应首部: 
```
$(document).ready(function(){  
    $.ajax({  
        url: "./test.php",  
        type: "POST",  
        data: {"user" : "min", "pass" : "he"},  
        error: function(jqXHR, textStatus, errorThrown) {  
            if (textStatus == "error") {  
                alert(textStatus + " : " +errorThrown);  
            } else {  
                alert(textStatus);  
            }  
        },  
        // 下面是获取响应首部
        success: function(data, textStatus, jqXHR) {  
            alert(jqXHR.getResponseHeader("Server"));  
            alert(jqXHR.getResponseHeader("Content-Type"));  
            alert(jqXHR.getResponseHeader("X-Powered-By"));  
            alert(jqXHR.getResponseHeader("Content-Encoding"));  
            alert(jqXHR.getAllResponseHeaders());  
            alert(jqXHR.getResponseHeader("Set-Cookie"));       //返回null，不能获取Set-Cookie的值  
            alert(data + textStatus);  
        }  
    });  
}); 
```
> jQuery通过XMLHttpRequest的getResponseHeader或getAllResponseHeaders()可以获取指定的HTTP header field的值，但规定不能获取Set-Cookie和Set-Cookie2的值。 