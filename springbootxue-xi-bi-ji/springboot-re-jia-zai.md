## springboot 热加载

> 在前端进行react开发的时候，代码都是可以热加载的，一旦代码进行修改，代码会重新编译，并及时更新反馈到浏览器当中，当然在eclipse中也有这样的功能，只需要在eclipse当中配置自动编译的功能。

- 添加spring-boot-detools依赖

在springboot项目的pom.xml文件中添加一下：

````xml
		<!-- 配置开发工具 可以自动重启 方便调试样式 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<optional>true</optional>
		</dependency>
````

此外还需要在maven的插件配置文件中进行配置，具体如下：

````xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<fork>true</fork>
				</configuration>
			</plugin>
		</plugins>
	</build>
````

> 