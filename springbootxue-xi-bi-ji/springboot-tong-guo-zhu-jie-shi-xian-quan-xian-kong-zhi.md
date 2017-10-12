## springboot 通过注解实现权限控制

- 让我们认识注解

springboot实现权限控制可以使用shiro,是的springboot可以帮我们完成很多的事情，其实这篇文章的重点是注解，但是单独的说注解没什么意思，每样技术，我更关心的是它能做什么，我们为什么需要它，那么注解是我们需要的么？我想，它确实是被需要的，因为在自己写项目的时候会用到很多的注解比如说@RequestMapping、@Override,所以我觉的我需要了解一下它。

- 注解是什么？

我们先不说注解具体的定义是什么？我们先打个比方，说一下注解像什么，我的理解啊！注解像个便利贴，@Override是个便利贴，这个便利贴，提醒我在编译的时候检查是不是实现的是接口中的方法，有了这个注解，这个类本身的活动并没有发生改变，这个类是个处理servlet的类，加了注解后他还是这个功能，没有变，但是他影响了别人，这个别人可以看做是编译这件事情，编译的时候编译器看到了这个注解。

- 为什么我们需要注解？

有没有发现注解和一个东西很像，xml配置文件，我们在使用springMVC这个框架的时候，常常会用到这样的一个注解@RequestMapping,这个组件有一个value属性，用于记录路由信息，

- 相关文章

[1][ Java 注解的作用与使用](http://blog.csdn.net/chenchaofuck1/article/details/52006961)
[2][Spring boot配置拦截器](http://blog.csdn.net/jiaobuchong/article/details/50394427)
[3][SpringBoot使用自定义注解实现权限拦截](http://www.jianshu.com/p/43c97352aa1e)
[4][理解Java基础之注解Annotation](http://developer.51cto.com/art/201107/276760.htm)
[5][java中自定义注解的作用和写法](http://m.blog.csdn.net/qq_36453032/article/details/53992961)
[6][Java 注解指导手册 – 终极向导](http://www.importnew.com/14227.html)
[7][JavaWeb学习总结(四十九)——简单模拟Sping MVC](http://www.cnblogs.com/xdp-gacl/p/4101727.html)