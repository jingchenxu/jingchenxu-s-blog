## springboot 配置swagger2

> 嘿！前后端分离开发的时候前端是不是觉得后端是死逼，接口开发成这鸟儿样还能用，关键是每次给的文档都不及时，马蛋字段加了也不说一声，如果有以上类似的情况，可以开始考虑使用swagger了。

springboot用起来就是爽，没错！一个字爽，简单的配置满足你的想象，在pom中加上下面的这些：

````xml
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger2</artifactId>
			<version>2.7.0</version>
		</dependency>
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger-ui</artifactId>
			<version>2.7.0</version>
		</dependency>
````

然后在入口文件中添加上一个注解就可以了：

````java
package com.jinhetech.lottery;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@SpringBootApplication()
@EnableSwagger2
public class LotteryApplication {

	public static void main(String[] args) {
		SpringApplication.run(LotteryApplication.class, args);
	}
}
````

但是需要注意的是，如果你的项目发布了，不希望别人看到这些，是需要设置这些页面需要账号及密码才能访问，需要在application.properties文件中添加相关代码通过spring.security来控制页面需要账户密码才能访问，其中比较关键的是，需要将多个路径设置为安全访问的路径：

````properties
security.basic.path=/manager,/swagger-ui.html
````

