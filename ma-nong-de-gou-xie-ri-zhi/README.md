# 码农的狗血日志

> 公司的主要技术站是springMVC+VUE,做了这么多些的项目后，发现总会有一些最佳实践沉淀下来，这或许就是所谓的套路。

* 要取一个好名字

代码也是门语言，写好代码就是要好好说话，好好说话就是要词要达意，在看很多的Java框架的源码时会发现他们的方法或变量的名称写的比较长，一般会是完整的单词，有的人会认为这是不好的，但我个人认为这让代码清楚的表达了意思，很多时候我们的业务代码是会被很多人接收的，如果名字没取好，会导致很多的问题。

取名字要有风格，如果是数组就加上List，在所有的代码中都按照一种风格写代码，有时候效果或许比注释好，但是注意不要轻易的改变风格，人的惯性思维是可怕的，要么都是这样，要么就都不是。

* 随时准备好处理异常

本来我写代码也是没有什么异常处理的概念的，但是经不起一些线上问题折腾，有些问题这又在测试的覆盖面宽泛到一定程度时才会发现，所以对可能发生异常的代码加上try,catch还是必要的，以前有一些不好的习惯，喜欢随便的加`System.out.println` ,这是有问题的，但是不加这个怎么调试呢？生产环境的问题追踪比较麻烦，所以我们可以在catch中加 `System.out.println`，将catch周围的变量都打印出来，这样只会在异常发生时会打印，而且可以将一些关键信息都打印出来。

* 构造好的bean

在Java中我们会构造很多的bean,那么一个好的bean的结构是什么样的呢？构建bean现在又很多的代码生成工具，主要的有生成 getter\setter, 生成builder模式相关的代码，生成 toString 方法，生成构造函数，以上就是我常用的，一般来说我的bean的类会继承于一个父类，这个父类会实现序列化接口，并

