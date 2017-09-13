---
layout: post
title:  "ajax + xml + servlet简单运用"
categories: JavaScript servlet
tags: JavaScript servlet java 
excerpt: 想信很多人学了ajax就有一个疑问，到底ajax传数据传到的是什么地址，而且传过去了之后后台是怎么处理的，而且后台是怎么传json数据过来的。这些呢，用文字是很难说清楚的，直接上代码
---


> 想信很多人学了ajax就有一个疑问，到底ajax传数据传到的是什么地址，而且传过去了之后后台是怎么处理的，而且后台是怎么传json数据过来的。这些呢，用文字是很难说清楚的，直接上代码。


```
js文件
function deleteBook() {
    var allInput = $(".main-form:eq(1) input");
    $.post("OperateServlet", {
      id: allInput.get(0).value,
      bookname: allInput.get(1).value,
      autor: allInput.get(2).value,
      price: allInput.get(3).value,
      method: "2"
    }, function(data) {
      var msg = data.msg;
      alert("删除成功");
    },"json");

    clearContent(allInput.filter("[type!=button]"));
  }
 
xml文件 web.xml
<servlet> 
    <description>各种操作的Servlet </description> 
    <display-name>OperateServlet</display-name>  
    <servlet-name>OperateServlet</servlet-name> 
    <servlet-class>control.OperateServlet</servlet-class> 
  </servlet>
  <servlet-mapping> 
    <servlet-name>OperateServlet</servlet-name> 
    <url-pattern>/OperateServlet</url-pattern>
  </servlet-mapping>
  
java文件 OperateServlet.java

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException
	{
		System.out.println("还是先看看有没有进OperateServlet的post");
		// 首先将编码类型都设置为uft-8
		request.setCharacterEncoding("utf-8");
		int tempCount = 0;
		
		// 获取method
		int method = Integer.parseInt(request.getParameter("method"));
		switch(method)
		{
		case 1 : addBook(request);break;
		case 2 : deleteBook(request);break;
		case 4 : alterBook(request);break;
		case 5 : tempCount=5;
		         queryAllBook();break;
		case 6 : borrowBook(request, response);break;
		case 7 : returnBook(request);break;
		case 8 : tempCount=8;
		         disBorBook(request);break;
		default: ;
		}
		
		if(tempCount == 3 || tempCount == 5 || tempCount == 8)
		{
			JSONObject jsonObject = new JSONObject();
			String msg = Protocol.msg;
			jsonObject.put("msg", msg);
			response.getWriter().write(jsonObject.toString());
			System.out.println("返回给服务器"+ msg);
		}
		
	}
```

>+ 分析：首先，js文件中ajax传的那个地址，即为xml中的servlet的名字,即<display-name>中的名字。然后，根据这个名字中对应的<servlet-class>位置(本例中这个control为java文件所在的包)，访问java文件。
>+ 其次：在java文件中通过request.getParameter()获取ajax传过来的参数，getParameter()中的参数就为ajax中的属性名字。
>+ 进行了一系列操作之后，用JSONObject jsonObject = new JSONObject();可以新建一个json对象，然后用put方法，将需要返回去的数据存在一个json属性中。
>+ 最后，通过response.getWriter().write(jsonObject.toString());将数据返回到客户端，由于write只能接受字符串，所以这里必须将json转化为string。
>+ 在js文件中，ajax的回掉函数的参数data就存储了服务器返回来的所有数据,所以用data.msg(msg为刚刚json创建的属性)就可以访问服务器刚刚的msg中的东西，然后就可以进行处理。