# python 开发环境配置

* 查看Python版本

个人使用的是Ubuntu16.04版本进行开发的，因为在Windows下做这些开发会比较的麻烦，所以最近把开发环境切换到了Ubuntu。

Python版本查看命令：

```bash
python
```

```bash
jingchenxu@jingchenxu-B85M-D3H:~$ python
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

* 添加Python版本管理工具

目前Python是存在2个版本的，很多的包目前还只支持Python2，为了开发方便我准备安装Python的版本管理器，这个和node的版本管理工具类似。

> !需要先安装git

```bash
git clone git://github.com/yyuu/pyenv.git ~/.pyenv
```

接下来请按照顺序执行以下代码：

```bash
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
$ exec $SHELL -l
```

查看Python可安装版本

```bash
pyenv install --list
```

安装最新版本的Python

```bash
pyenv install 3.6.0 -v
```

> 检测是否安装成功（安装完成后并没有就可以直接使用）

设置使用版本

```bash
pyenv global 3.6.0
```

返回系统默认使用版本

```bash
pyenv global system
```

