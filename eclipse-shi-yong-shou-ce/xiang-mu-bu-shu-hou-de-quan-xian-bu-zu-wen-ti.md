# 项目部署后的权限不足问题

* 项目权限不足 通常情况下我们是通过war包发布项目的，war包发布到tomcat后进行自解压，会解压出一个项目文件夹，但是我们需要注意的是，真正在tomcat中执行的代码是在work这个文件夹当中的，不是webapp文件夹当中的，所有项目没有执行权限的时候，我们要看一下work文件夹下的项目文件夹所属的组及用户。
* 项目需要操作的目录权限不足

在项目中我们有时需要保存文件，有时候保存文件失败，就是应为该文件夹的权限不足导致的。

对于tomcat中部署项目时出现的权限不足问题，目前总结出以上２点原因。

