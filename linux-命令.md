- 删除命令

```linux
sudo rm -f sample.txt
```

- 移动文件命令

```linux
sudo rm sample.txt sample1.txt sample2.txt position
```

- 查找端口占用进程

```shell
netstat -nlp|grep 8080
```

- 强制杀死占用端口进程

```shell
kill -9 27738
`
```

- 安装nvm

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
```

- 给文件夹下的所有文件赋予权限

```shell
chmod 777 * -R
```

- 给单个文件赋予权限

```shell
chmod 777 filename
```

- 修改文件所属组

```shell
sudo chown username *
sudo chgrp username *
```

- 删除文件夹

```shell
rm -rf /file/file
```

- 查找指定的文件或名称

````shell
find ./ -name filename
````



