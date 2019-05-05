# 创建 flask 应用

> 为什么要使用flask,最近在做电商的可视化，就是电商数据图表统计，写了一堆的SQL，于是就在想，有没有什么开源的图标生成工具，然后生成iframe嵌入到现有系统当中，其实BI软件，后来找到了一个superset,airbnb的，整体使用起来很简单，看了一下，其使用的技术栈中存在flask,感觉用flask学python也不错。

* 使用pipenv

java有maven,javascript有npm,对应的依赖文件有pom.xml,package.json，那么python项目使用什么来管理项目依赖的呢？答案是pip,pip只能算是python安装工具，pipenv是为每个项目创建一个运行环境，感觉和docker有点像啊！pipenv通过配置文件初始化好项目运行环境，前提是你要装好pipenv。

使用pycharm创建时不错的选择，使用pycharm创建项目时可以设置是否使用pipenv,也可以选择当前系统中存在的pipenv环境，

