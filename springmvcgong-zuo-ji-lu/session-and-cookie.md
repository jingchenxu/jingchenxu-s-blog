# session & cookie

* cookie

具体翻译我就不再复制粘贴百度百科了，我只说说我在平时的使用，cookie可以作为浏览器端临时数据的保存方式，由于保存在浏览器端，所以安全性不是很高，我们在浏览器的开发工具中能够很轻易的看到有哪些cookie，cookie值是什么。

就我个人而言，目前主要是在类似购物车这一类的模块中会使用到cookie,但是cookie的使用存在一些问题，各个浏览器对cookie的大小有一定的限制，而且不同浏览器的限制还不尽相同，此外对单个应用下可写入的cookie的条数也存在一定的限制，如果你的cookie过大，那么你的http的请求头就会过大，在某些情况下，会导致请求失败。因为tomcat nignx 之类的web服务器对http请求头的大小是有限制的。

* session

由于http请求是无状态的，想要记录某次http请求的状态，怎么办，将http请求的状态放在cookie当中，但是cookie存在诸多的限制，而且客户端存在不可控性，很多的敏感信息不能存储。

个人认为session是cookie的延申，session如何判断请求的状态还是通过cookie,当客户端发起一个请求的时候，我们会在客户端的cookie当中生成一个cookie,JSESSIONID,JSESSIONID就相当与客户端打开session的钥匙，由于session是存放在服务端的，客户端是无法判断session中存储的信息是什么，session中可以存放用户的基本信息，这样客户端每次请求资源的时候，就可以从session中取得用户的信息，而不需要到数据库中进行查询。

session也存在一些问题，session在做负载均衡的时候处理起来比较的麻烦，此外，如果在session中存储过多的信息，也会占用大量服务端的资源。

* 总结

session 与 cookie都是为了保存状态，方便访问，减少不必要的数据库操作，session 和 cookie 基本都可以全局访问，使用方便。

* 相关文档

[log4j.properties配置详解与实例-全部测试通过](http://blog.sina.com.cn/s/blog_5ed94d710101go3u.html)

