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

> 以上配置完成后，maven会自动下载所有的jar包，但是如果你的网络抽风导致下载失败，在intellij中会出现如下的提示```Failed to read artifact descriptor for xxx:jar ``` 这是由于你的jar包下载失败导致的，这个时候我的解决方案就是到.mvn文件夹中找到对应的jar包，在对应的文件夹中你会看大未下载完成的文件，你需要做的就是删除此文件夹，然后重新下载。

- 在application.properties文件中配置

主要的关于spring-boot-devtools配置文件如下：

````properties
## spring dev-tool配置

#spring.devtools.livereload.enabled=true
spring.devtools.restart.enabled=true
spring.devtools.restart.exclude=static
spring.thymeleaf.cache=false
spring.devtools.restart.additional-paths=src/main/java
````

- 设置intellij

你以为以上的工作做完就就结束了么，没有，如果你使用的是intelij,你还需要另外的配置。

首先File->Settings->Build,Execution,Deployment->Compiler

![intellij配置](/img/springboot/springboot2.png)

此外还需要以下的配置

按下Ctrl+Shift+Alt+/ 组合键，选择Registry选项，后勾选下图所示的选项：

![设置立即编译](/img/springboot/springboot3.png)

