## git daily

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

 1. 