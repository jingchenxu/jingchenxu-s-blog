## springboot 使用 mybatis-generator

- mybatis-generator解决了什么

一个项目开始的时候，除了搭建项目框架，其他的比较麻烦的就是创建数据库表对应的java bean，以及穿件对应的dao层，编写这部分的文件会占据一个项目大量的时间，这些代码或文件都是可以使用插件生成的。

- 如何使用mybatis-generator

1. 修改pom配置文件

mybatis-generator是maven的一个插件，所以我们需要在pom的配置文件当中添加mybatis相关的配置，具体的配置如下：

````xml
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.6</version>
                <dependencies>
                    <dependency>
                        <groupId> mysql</groupId>
                        <artifactId> mysql-connector-java</artifactId>
                        <version> 5.1.39</version>
                    </dependency>
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.3.6</version>
                    </dependency>
                </dependencies>
                <executions>
                    <execution>
                        <id>Generate MyBatis Artifacts</id>
                        <phase>package</phase>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <!--允许移动生成的文件 -->
                    <verbose>true</verbose>
                    <!-- 是否覆盖 -->
                    <overwrite>true</overwrite>
                    <!-- 自动生成的配置 -->
                    <configurationFile>
                        src/main/resources/mybatis-generator.xml</configurationFile>
                </configuration>
            </plugin>
````


