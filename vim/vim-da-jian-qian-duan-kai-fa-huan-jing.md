# vim 搭建前端开发环境（1）

## windows10 开启linux子系统

之前是迫于服务器环境的原因，简单的了解了下vim，在能够满足在服务端简单的使用vim修改文件之后，便很久没有仔细的研究vim，不打算再windows下使用vim，说不上什么理由，就是感觉不对，最近发现win10下的Linux子系统变得十分的完善，安装之后体验比较好，打算把开发环境迁移到子系统当中。

首先我们需要开启win10最linux子系统的支持，开启支持需要2个步骤，分别为开启开发者状态，开启对Linux子系统支持的特性：

第一步：我们通过点击Cortana，在对话框的下方的输入框中输入“开发”搜索关键字，可以看到最佳匹配就是“开发者选项设置”，点击后直接进入开发者选项的设置页面，选择开发人员模式：![](../.gitbook/assets/tim-tu-pian-20180621101240.png)接下来我们需要开启对Linux子系统支持的特性，搜索“启用或关闭windows功能”，最匹配项为“启用或关闭windows功能”，进入设置：

![](../.gitbook/assets/tim-tu-pian-20180621102302.png)

选中适用于Linux的Windows子系统，至此前期的系统配置已经完成，接下来我们从微软商店下载安装Ubuntu系统，

在微软商城查找

![](../.gitbook/assets/tim-tu-pian-20180621102609.png)安装完成后进入Ubuntu命令界面，通过sudo passwd root设置系统管理员账号密码，通过apt-get-update apt-get-upgrad命令更新系统。

