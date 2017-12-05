## 线程安全

- 如果线程不安全

之前做过一个前后端分离的项目，项目的首页会调用到多个接口（接口代号分别为A、B、C），这个时候出现了这样的一个问题，首页的接口是同时调用的，应为ajax是异步的，所以接口的调用应该是部分先后的，于是就出现了这样的问题，A接口中返回的是B接口应该返回的内容。

项目使用的是springMVC框架，这三个接口对应的是在一个被controller注解的类下的三个方法，项目为了统一返回数据的格式，创建了一个ReturnValue类，A、B、C三个接口的三个方法使用的返回数据的对象都是在方法外初始化的，也就是说ReturnValue 创建的对象rtv可以看做是三个接口的全局变量。

- 关于servlet

我们现在都在间接的使用servlet,但是不管如何开发web程序我们都在使用servlet,

- 还原一下问题

下面的这段代码不是线程安全的，我们可以看到，在页面中显示的数字都是3

````java
package test;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class TestServlet
 */
@WebServlet("/TestServlet")
public class TestServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	public int i = 1;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public TestServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		// int i = 1;
		i++;
		try {
			Thread.sleep(1000*4);
		} catch (InterruptedException e) {
			// TODO: handle exception
			e.printStackTrace();
		}
		response.getWriter().append("Served at: ").append(request.getContextPath());
		response.getWriter().append(String.valueOf(i));
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}
````

这里我们先简单的介绍一下servlet,我们的应用可以看做是一个线程，是运行在web容器当中的，我们最常见的web容器（servlet容器）就是tomcat,应用启动后，servlet容器会处理来自外部的请求，