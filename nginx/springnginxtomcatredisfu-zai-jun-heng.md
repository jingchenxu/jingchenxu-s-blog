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

以上的方式存在一定的问题，