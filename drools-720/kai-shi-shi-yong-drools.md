## 开始使用Drools

- 创建一个规则包

我们在project->src->main->resources文件夹下创建一个规则包，创建完后，我们需要在配置文件中生命一个该配置包的session，配置文件所在的目录为project->src->main->resources->META-INF->kmodule.xml,Drools jar包默认从此路径的的此文件中读取此配置XML文件。

配置示例：

 ````xml
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
    <!-- 订单保存时间的价格校验规则 -->
    <kbase name="ordconfirmrules" packages="orderconfirmrules">
        <ksession name="ksession-orderconfirm"/>
    </kbase>
</kmodule>
 ```` 
 
- 创建调用规则的帮助类

