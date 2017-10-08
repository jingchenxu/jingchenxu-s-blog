## springboot 配置pom

pom应该是maven的配置文件，用于管理项目的jar包，这样可以无需将项目的jar包存放到项目的bin文件目录下，在提交代码的时候你是提交呢还是不提交呢？提交的话在git会存在加大的文件，如果你不提交的话，别人又不知道你添加了jar包。

springboot中的各个jar包的版本是如何确定的呢？我们可以通过继承或导入的方式来确定各个依赖jar包的版本。

- 方案一（继承）

````xml
<project>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.1.9.RELEASE</version>
  </parent>
</project>
````

- 方案二（导入）

````xml
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>1.1.9.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
````

有时候我们会看到在别人的项目中会制定某个jar包的版本，但是需要注意的是，只有在继承的方式下才有效，采用导入的方式是无效的。

我们可以看到，jar包的依赖主要有这样的几个字段，分别是groupid、artifactid字段，groupid表示的是项目组织的唯一标识，同时也对应了java包下的项目结构，而artifactid表示的是该项目的唯一标识符。

