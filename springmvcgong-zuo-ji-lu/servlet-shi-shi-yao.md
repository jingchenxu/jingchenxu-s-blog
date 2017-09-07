## servlet是什么

- 关键词

*servlet容器*，*servlet实例*，*线程*，*进程*，"HttpServlet"

- 为什么想知道servlet是什么

问题一开始是我想知道从客户端发请求到服务端是如何处理的，首先我们的请求是发送到指定端口的，那么在这个端口肯定有一个人在哪里等待着...

- 简述流程

在servlet技术之前，使用的是什么技术呢？在这之前使用的是CGI（Common Gateway Interface）技术，CGI和servlet技术都是处理客户端请求的技术，他们根据请求，返回不同的请求资源，早起可能是返回html比较的多。

CGI和servlet的区别又在哪里呢？CGI是运行时时作为一个进程存在的，而servlet是作为线程存在的，servlet的对象本身不存在main方法（servlet 及 servlet容器都是java实现的），在servlet中是存在main方法的，那又要问了servlet容器在哪里？servlet容器是由webserver实现的，比如tomcat。

如果以tomcat为例，我们来了解一下，一个客户端请求在服务端处理的过程：

1、 tomcat 启动监听8080端口
2、 tomcat 收到一个发向8080端口的请求
3、 tomcat 调用servlet容器处理
4、 servlet 调用servlet类创建一个实例开启一个线程处理
5、 servlet 实例处理完成后，将结果交给servlet容器，然后次线程结束
6、 servlet 容器将结果在交给tomcat

进程可以看做是一个应用程序，在window下，我们在任务管理器中可以看到各种各样的进程，基本上一个应用对应一个进程，也就是说进程在应用的整个生命周期都是存在的。那么进程将长期的占用系统资源；而线程则不是这样的，线程可以出现在应用生命周期的各个时间段，同一个时间段可以有多个线程，而且线程占用的系统资源相对较少。

- 我们来应用一下

java框架的使用，让我们离底层的实现原理越来越远，如果是使用SpringMVC这样的框架，实现一个请求接口很简单，只需要使用@Controller、@RequestMapping注解即可实现。

但如果我们想通过一个servlet类来实现这样的一个接口呢？

目前在一些代码中仍然会看到这样的代码，并没有使用框架而是使用实现servlet类来实现接口功能，那我们就来看一下，具体如何实现：

````java
package testhttpservlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {

	private static final long serialVersionUID = 1L;

	 public void doGet(HttpServletRequest request,
	  HttpServletResponse response)throws IOException,ServletException{
	  //请求http://localhost:8080/DroolsTest/hello？clientName=me 访问
	  String clientName=request.getParameter("clientName");
	  if(clientName!=null)
	   clientName=new String(clientName.getBytes("ISO-8859-1"),"GB2312");
	  else
	   clientName="我的朋友";

	  //第四步：生成HTTP响应结果
	  PrintWriter out;
	  String title="HelloServlet";
	  String heading1="HelloServlet的doGet方法的输出：";
	  //set content type
	  response.setContentType("text/html;charset=UTF-8");
	  //write html page
	  out=response.getWriter();
	  out.print("<HTML><HEAD><TITLE>"+title+"</TITLE>");
	  out.print("</HEAD><BODY>");
	  out.print(heading1);
	  out.println("<h1><p>"+clientName+":您好</h1>");
	  out.print("</BODY></HTML>");

	  out.close();
	 }
}
````

web.xml配置如下

````xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" version="3.0">
  <display-name>DroolsTest</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
   <servlet-name>HelloServlet</servlet-name>
   <servlet-class>testhttpservlet.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
   <servlet-name>HelloServlet</servlet-name>
   <url-pattern>/hello</url-pattern>
</servlet-mapping>
</web-app>
````


相关代码可见[github](https://github.com/jingchenxu/drools-lesson)

- 参考

[HttpServlet详解](http://www.cnblogs.com/panjun-Donet/archive/2010/02/22/1671290.html)
