## 如何生成ssh key

> git是通过ssh来传输的，需要再本地配置ssh key,方便ssh的各项操作。

- 配置相关信息
````bash
git config --global user.name "yourname"
git config --global user.email "youremail"
````

- 看看有没有生成ssh key

````bash
git cd ~/.ssh
````

- 生成ssh key

````bash
ssh-keygen -t rsa -C "jingchenxu2015@gmail.com"
````

- 查看ssh key

.ssh文件夹默认是隐藏的，.ssh文件在home文件夹下，但是默认是隐藏的，通过 Ctrl+h 键显示隐藏文件夹。

使用文本编辑器打开id_rsa.pub,可查看到公钥，将该公钥添加到你的git仓库账户即可。
