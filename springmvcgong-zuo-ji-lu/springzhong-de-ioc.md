## spring中的IOC

这个概念被念叨很久了，我也想知道spring中的IOC（控制反转）到底是什么，DI(依赖注入)是什么？这是个很蛋疼的问题，百度一下，清一水的“解耦”，百度出了各种各样的解释，看来大家对它还是很关注的。“解耦”不知道能不能理解为“兼容”，对于我们不清楚的东西，我们最好尝试用熟悉的东西打个比方可能会更好。

IOC在写代码中最明显的体验是什么，少些了很多new，new是用来创建对象的，java这样的面向对象语言，没了对象怎么办，我们还需要对象，只是我们不在使用new来创建对象了，我们交给spring来创建对象，我们需要对象的时候，只需要向spring要就可以了。

你会感觉到这确实很方便啊，是的，但是你要为spring准备好一些东西你才可以这么方便，那就是你需要向spring注册你需要控制反转的类，你可以通过XML进行注册，大致的代码如下：

````xml
<bean id="userService" class="org.leadfar.service.UserService"/>  
<bean id="documentService" class="org.leadfar.service.DocumentService"/>  
<bean id="orgService" class="org.leadfar.service.OrgService"/>  
  
<bean id="userAction" class="org.leadfar.web.UserAction">  
     <property name="userService" ref="userService"/>  
</bean>  
````

这样你就完成了注册，在这里你注册了类名，spring通过该类名在java反射的帮助下就可以创建一个该类的实例，这点我是能理解的，但是我发现自己的项目里好像没有并没有这样的XML文件啊！那spring是如何实现IOC的呢？

这是另外一个技术，动态代理，关于动态代理，我这里先不展开，但是动态代理只支持接口（其对象的创建方式也是利用反射），所以我们在使用springMVC时，service层使用的是接口，然后再实现接口。

有人说用DI这个名字可能会更好理解，确实是这样的，A类中需要调用B类，我们可以理解为A类依赖于B类，过多的依赖会让类与类之间形成网状的结构，结果就是代码高度耦合，spring的IOC就是用于解决类与类之间的依赖关系的，A类如果依赖B类，A类会向spring申请B类的实例（其实好像没有申请，A需要B，spring就会向A提供B），也就是说A、B之间不存在耦合了，每个类都处于同一个层级。

- 相关文章

[谈谈Spring中的IOC和AOP概念](http://blog.csdn.net/eson_15/article/details/51090040)
[透透彻彻IoC（你没有理由不懂！）](http://stamen.iteye.com/blog/1489223/)
[Spring 的本质系列(1) -- 依赖注入](https://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665513179&idx=1&sn=772226a5be436a0d08197c335ddb52b8#rd)