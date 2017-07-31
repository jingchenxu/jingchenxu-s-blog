## 如何写好gitcommit

- 添加git commit模板

首先我们需要创建一个git commit的模板，创建完成后，将该模板配置到git当中，然后在每次键入git commit命令的时候，会自动的调用该模板，根据实际的情况修改该模板中的内容，最后进行提交。

创建模板文件gitcommit_template:

````
commit类型：bug修复、功能添加、功能优化
commit内容：
关联的需求id:
关联的BUGid:
````

设置全局的提交模板：
````
git config --global commit.template gitcommit_template
````

设置打开该模板的文本编辑器：
````
git config --global core.editor vim
````

提交代码
````
git commit (git gommit 之前需要将没有加入代码库的 git add 进入代码库)

git commit -a (这个可以提交多个代码文件)
````

- 通过commitizen进行交互式的commit提交

想想看我这样的需求肯定是由轮子的，果然找到一个，配置简单舒适：

````
npm install -g commitizen
npm i -g cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
````


以上三步设置完成后，只需要git cz即可

![gitcommit](/img/git/gitcommit.gif)

