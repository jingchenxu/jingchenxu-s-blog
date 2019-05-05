# 让我们认识drools

* 简介

Drools是什么呢？百度一下你会找到不多的资料，相比于其他的Java框架来说。Drools是规则引擎，好吧就说有什么用吧！

举个例子：在线商品会有较多的折扣，有的是正对整张订单的折扣，有的是某段时间针对某个商品打折，有的是针对达到某个数量的商品进行打折... 蛋疼了没有？崩溃了没有？如果有，那就对了。

如果有代码来实现这些业务的话，没错是可以的，但是由此带来的问题也是相当可以的。

这时候就是我们的规则引擎出场了？有没有在流水线上干过，我有，我们把需要的处理的数据看做是需要进行一系列工序的原材料，而Drools可以看做是流水线，大致明白是什么意思了吧！

* 相关资料

1、 官方网站：[www.drools.org](http://www.drools.org/)

2、 相关大神的博客：[http://my.csdn.net/lifetragedy](http://my.csdn.net/lifetragedy)

* demo相关

首先我们可以简单的感受下Drools,可以通过什么来感受呢！，你需要安装一下Drools相关的插件，具体的操作过程可以查看[https://nheron.gitbooks.io/droolsonboarding/](https://nheron.gitbooks.io/droolsonboarding/),这个是Drools最新的相关文档（2017-08-31），如果是之前没有接触过Drools可以按着教程敲完Drools tutorial章节中出现的代码。

需要注意的是，如果你是用Drools插件生成的项目，那么按照教程你的Drools Library应该是没有问题，但是如果你是在现有的项目中集成Drools的话，我还是建议使用最新的版本的，虽然市面上有很多的6.0版本的教程，但是7.2.0的版本真的很方便，我们需要注意下，Drools Library引入的是否正确，如果项目部署到了Tomcat当中的话，还需要注意在Tomcat的lib文件夹中是否引入Drools相关的jar包。

我自己正在讲官方提供的代码敲一遍，其中某些地方加了一些中文注释：具体的至为：[Drools-lesson](https://github.com/jingchenxu/drools-lesson)

