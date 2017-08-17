## springboot 使用 thymeleaf 模板引擎

- 配置pom

在pom中配置一下代码：
````xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
````

- 配置application文件properties

````xml
# 禁用thymeleaf缓存
spring.thymeleaf.cache=false
# 其他配置
spring.thymeleaf.prefix=classpath:/templates
spring.thymeleaf.suffix=.html  
spring.thymeleaf.mode=HTML5
spring.thymeleaf.content-type=text/html  
````