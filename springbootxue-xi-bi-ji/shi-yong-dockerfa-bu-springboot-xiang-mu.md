## 使用 docker 发布springboot项目

- 一种最简单的方法

对于springboot来说使用docker来部署项目是非常方便的，我们可以找到一个叫 [docker-maven-plugin](https://github.com/spotify/docker-maven-plugin) 来完成这件事，集成方法非常的简单，在pom的插件节点添加这个节点就可以了，具体的配置文件如下：

````xml
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.0.0</version>
                <configuration>
                    <imageName>daisy</imageName>
                    <baseImage>java</baseImage>
                    <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>
                    <!-- copy the service's jar file from target into the root directory of the image -->
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>
                </configuration>
            </plugin>
````

配置简单如斯，下面我们 ````mvn docker:build````命令，即可完成镜像的生成工作，需要注意的是，镜像是生成在docker当中的，这就需要你本机安装docker，而且很有可能你的镜像生成会报错，会报一个2375端口无法访问的错误，这个错误的原因是，这个端口原本是作为远程管理docker的端口，但是该端口被暴露存在风险，所以目前该端口被默认关闭了，需要我们手动打开，打开的方式为：

![](/img/springboot/docker.png)

端口开启后，你会看到应该就没有问题了，镜像会顺利的被发布，看一下：

![](/img/springboot/daisy.png)

接下来运行并创建容器

````bash
docker run --name daisy -p 5000:4000 -d daisy
````



