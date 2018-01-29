## 如何使用docker创建并发布一个服务镜像

> 最近几个idea的注册服务器好像是挂了，别人的服务器果然是不靠谱的，所以打算自己搭建一个，想到最近在学习docker所以就想着，制作一个注册服务的镜像也是极好的。

- 准备镜像资源

由于学习docker还不够深入，目前对docker的应用还停留在，使用sh脚本作为docker入口的阶段，所有的docker容器内部的部署都是用sh脚本完成的。

准备资源如下：

![](/img/docker/dockerimage.png)

相关的资源可见地址[ideareegister](https://github.com/jingchenxu/idearegister),在该地址下载相关的资源后，便可进行镜像的制作工作。

- 镜像制作过程

````bash
docker build -t idearegister:latest .
docker run --name idea -p 2048:2048 -d idearegister
docker commit idea deepwater2017/idearegister
docker login
docker push deepwater2017/idearegister:latest
docker pull deepwater2017/idearegister
docker run --name idearegister -p 2048:2048 -d idearegister


````

