# gitlab部署域名到期

## 简介

公司使用gitlab进行代码管理，该服务器配置了域名（部署在阿里云上），昨天gitlab突然没法访问了，而且页面完全没有报错，打开chrome调试看到以下内容：

![](../.gitbook/assets/gitlab.png)

点击错误中的链接，跳转到该页面：[阿里云服务到期页面](https://www.aliyun.com/act/aliyun/parking/)

## 解决

生命还要继续，代码还要提交

1、在项目的.git文件下找到config文件 2、修改\[remote "origin"\]，替换其中的原来的域名为IP地址（IP地址后无需添加端口号）

