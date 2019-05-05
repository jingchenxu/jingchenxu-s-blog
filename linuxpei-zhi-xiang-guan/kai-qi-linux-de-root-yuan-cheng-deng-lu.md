# 开启Linux的root远程登录

* 设置root密码

首先需要设置你的root密码：`sudo passwd root`

* 设置允许root账户ssh

```text
vi /etc/ssh/sshd_config
```

注释掉下面的这段话

`PermitRootLogin without-password`

添加下面这段话

`PermitRootLogin yes`

* 重启ssh

`service ssh restart`

