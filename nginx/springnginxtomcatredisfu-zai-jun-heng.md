## nginx负载均衡，tomcat共享session

- 背景

在一些特定的项目背景下，要求系统24小时不宕机，但是期间还要能够上线一些新的功能，做到热更新，又或是项目的并发较高，需要进行负载均衡时，其实如果是简单的负载均衡，nginx已经为我们提供的丰富的方式。

- nginx负载均衡基本配置

首先说一种没有贡献session的负载均衡的方式，我们可以指定来自同一客户端的用户每次访问的tomcat为固定tomcat，具体的配置如下：

````config
upstream backserver { 
ip_hash; 
server localhost:8080; 
server localhost:8081; 
} 
````

以上的方式存在一定的问题，只能说是实现了最基本的负载均衡，用户一开始访问的是8080端口，那么他的session就是存储在8080端口的这台tomcat当中的，如果你这是关闭这台tomcat，nginx会自动的指向8081端口，但是问题来了，tomcat默认是将session存储在内存当中的，tomcat一旦关闭，那么他的session就消失了，这就是为什么我们本地开发的时候，tomcat重启后，客户端需要重新登录，采用这种负载均衡的方式就会出现这样的问题，任意一台tomcat下线，都有可能导致session丢失。

- redis存储session

上一种方案最后的问题在于session是存储在内存当中的，另外一个tomcat无法获取当前tomcat的session，那我们就要解决这个问题，那就是将session放在一个俩个tomcat都可以访问的地方，