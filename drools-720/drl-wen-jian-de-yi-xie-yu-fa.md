# drl 文件的一些语法

* 规则文件的结构

drl文件大部分是由以下几个部分的组成的：包名、导入的外部类、声明的全局变量、规则。

我们先写一个简单的规则：

```java
rule "Your First Rule"

    when
        //conditions
        eval(true)
    then
        //actions
        System.out.println("show me good message");

end
```

我们可以看到这样的几个关键字 rule、when、then、end

