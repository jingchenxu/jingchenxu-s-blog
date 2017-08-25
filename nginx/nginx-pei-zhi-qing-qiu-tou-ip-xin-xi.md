## nginx配置请求头IP信息

- 出现的问题

最近在进行微信h5支付功能的开发是，在调用微信的统一支付接口的时候，需要支付发起客户端的ip地址，这个IP地址应该是外网的ip地址，但是日志中显示的是127.0.0.1，why?

- 分析

分析什么      直接百度，

- 结论

后台是如何获取到客户端额ip的，该项目后台使用的是java开发的，在controller层可以通过HttpServletRequest对象获取到客户端的ip,你的请求来自何方，请求中就会带来什么样的ip;

服务器开启了nginx，服务端是通过代理来处理向服务端的请求的，那么我们的应用程序接收到的请求是不是来时nginx，nginx和应用部署在同一台服务器，所以127.0.0.1是可以理解的。

怎么办，那就需要把经由nginx转发之前请求的ip带过来啊！这个就需要设置了，这个需要在nginx配置文件中进行配置。

配置如下：

````conf
location ~ ^/payapitest/  
{  
index index.action index.jsp;  
proxy_set_header x-forwarded-for  $remote_addr;  
proxy_pass http://localhost:8080;  
}  
````
````java
    public String getIpAddr(HttpServletRequest request) {

    	String ip = request.getHeader("x-forwarded-for");

    	if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {

    	ip = request.getHeader("Proxy-Client-IP");

    	}

    	if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {

    	ip = request.getHeader("WL-Proxy-Client-IP");

    	}

    	if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {

    	ip = request.getRemoteAddr();

    	}

    	return ip;

    	}
````