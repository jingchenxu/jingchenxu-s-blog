## git daily

> 本文主要作用为归纳一些常用的git开发操作，具体的学习查看[廖雪峰的教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，日常开发中git使用的问题还不止于此，git merge 就会产生较多的问题，这些问题不包含在此教程当中，最好的学习方法还是自己多试几次，可以在github上创建个项目进行这些测试。

- 加入项目

 1. 获取你的项目（基本上项目周期里只要一次）

    ````bash
    git clone git@xxx.xxx.xxx.xxx:jcxu/County.git
    ````

 2. 进入项目目录

   ````bash
   cd County
   ````

 3. 创建自己的分支

   ````bash
   git branch jcxu
   ````

 4. 将自己本地创建的分支推送到远程

   ````bash
   git push --set-upstream origin jcxu
   ````

- 每天晚上下班前代码写好

 1. 保存代码
 
   ````bash
   git add .
   ````
   
 2. commit 代码
 
   ````bash
   git commit -m --save
   ````
   
 3. 推送代码至服务器
   
   ````bash
   git push
   ````
   
- 每天早上开始写代码

 1. 获取所有分支的最新代码
 
   ````bash
   git fetch
   ````
   
 2. 合并master分支的最新代码(当然你也可以合并其他的分支)
 
   ````bash
   git merge origin/master
   ````
   