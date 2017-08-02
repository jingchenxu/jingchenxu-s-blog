## intellij 创建一个springboot项目

> 目前前端工程化相对来说已经比较的全面了，在npm和各类cli的帮助下，初始化一个前端项目变得越来越容易，前端目前主要的框架有React、Vue、Angular,如果不通过cli的话，对于项目模板可通过npm根据package.json,完成项目初始化，不管是通过模板项目或是cli前端的工程化都可以做到开箱即用，尤其在npm@4.0和yarn中，可锁定包的依赖版本后，前端工程化个人觉的已经相对比较完善了，在java项目中如果不使用maven,那么对于项目依赖包的管理是非常麻烦的，刚接触spring框架的时候，xml一堆，感觉非常的繁琐，我相信应该不是我一个人有这样的问题，果然就有了springboot这个框架，spring的配置比较的繁琐，所以springboot造了很多starter,而且还有http://start.spring.io 这样的工具，解决了很多spring配置的问题，但是这些可能会不利于我们理解spring。

- 我们用eclipse初始化一个springboot项目吧

首先我们在eclipse的插件市场找找到这个插件：spring Tools插件，安装完成后就可以新建一个springboot项目

![](/img/springboot/springboot1.gif)

可以看到这边的项目生成实际还是通过http://start.spring.io 来实现的。

- 通过intellij生成一个springboot项目

最近发现intelij无比的强大，遂开始积极的使用，首先我们先生成一个项目，我们会选择一个项目最基本web，这里是集成了springMVC的，此外还有数据库驱动，数据库持久化工具mybatis。


![](/img/springboot/springboot2.gif)