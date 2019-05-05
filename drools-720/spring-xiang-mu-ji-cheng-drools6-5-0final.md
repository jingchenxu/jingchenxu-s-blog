# spring 项目集成Drools6.5.0Final

* 开始

是的，公司的项目就是maven也没有用，就是一个eclipse生成的普通项目，然后倒入spring相关的jar包，自己搭建的一个框架。这样的一个项目因为项目的推进需要集成Drools。

* 为什么选择6.5.0Final版本

网络资源多？不是，而是不得不妥协的问题，目前项目的测试及生产环境的JDK版本都是1.7,而Drools7.0.0 7.2.0 折2个版本都是由1.8的JDK编译而成的，我们可以通过验证class的版本来确认，如果你强制用高版本的Drools,那么你的的项目会出现这样的问题，Unsupported major.minor version 52.0，这是低版本的jvm无法加载高版本的JDK编译的class文件导致的。

对于7.0.0Final版本的Drools中的class的版本进行校验，校验的方法如下，在class文件的根目录下查看class的版本： ![&#x56FE;&#x7247;](../.gitbook/assets/javaversion%20%281%29.png) 我们主要查看的信息是 major version:52 52标识的是由JDK1.8编译生成的class。

* 如何集成

其实最简单的方法可以是将Drools相关放到WEB-INF的lib文件夹当中，当然也是可以添加一个jar library,但是需要注意的是，在打包为war包是，需要将该library添加到最后发布的war包当中，具体设置如下： ![&#x56FE;&#x7247;](../.gitbook/assets/class.png) 如图图中的JUnit 4library是会被添加到最终发布的war包的WEB-INF的lib文件夹中的。 当然你也可以将drools相关的jar包都存放到WEB-INF的lib文件夹当中，其实使用drools就是引入drools相关的jar包然后进行调用，这只是最简单的用法。

* Drools执行的流程

如果项目中集成了drools,关于drools的相关的配置XML文件存放的位置默认是在src/main/resources/META-INF/kmodule.xml文件当中，我们可以看下配置文件的内容：

```markup
<?xml version="1.0" encoding="UTF-8"?>
<kmodule xmlns="http://jboss.org/kie/6.0.0/kmodule">
    <kbase name="rules" packages="nestlerules">
        <ksession name="ksession-rules"/>
    </kbase>
    <kbase name="rules2" packages="nestlerules">
        <ksession type="stateless" name="ksession-rules-stateless"/>
    </kbase>
    <!--  订单确认页规则 -->
    <kbase name="rules1" packages="checkrules">
        <ksession name="ksession-rules1"/>
    </kbase>
    <!--  计算订单中每个商品的realprice -->
    <kbase name="countrealpricerule" packages="countrealprice">
        <ksession name="ksession-countrealprice"/>
    </kbase>
    <!-- 订单保存时间的价格校验规则 -->
    <kbase name="ordconfirmrules" packages="orderconfirmrules">
        <ksession name="ksession-orderconfirm"/>
    </kbase>
</kmodule>
```

目前我只使用drl格式的规则文件，在配置文件中我们指定了好几个规则，每个规则有独自的name,package\(对应的规则文件包名\)，每个规则对应的session名称，session作为你的程序调用规则的通道，第一次调用的时候比较的缓慢，但是之后的速度就比较快了。

* 出现中文乱码怎么办

Drools运行时的编码使用的是系统的默认编码，所以需要注意你的系统的默认编码是什么，此外还需要注意你的文件编码是什么，一般最好设置你所有的编码格式为UTF-8，网上搜一下，Drools中文乱码的原因，我找了一下大概还要一种可能没有涉及到，那就是eclipse的工作空间的编码格式，如下图： ![&#x56FE;&#x7247;](../.gitbook/assets/drools6.png)

* 备注

Drools各版本下载地址：![Drools&#x4E0B;&#x8F7D;&#x5730;&#x5740;](http://download.jboss.org/drools/release/)

