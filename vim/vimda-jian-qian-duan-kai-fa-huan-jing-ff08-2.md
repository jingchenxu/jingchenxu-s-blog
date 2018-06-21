# vim搭建前端开发环境（2）

### 通过nvm安装node

nvm的开源地址为[https://github.com/creationix/nvm，](https://github.com/creationix/nvm)使用 

```bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bashwget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

命令进行安装，键入nvm可以查看nvm相关的命令，荣国nvm install v8.00可以安装对应版本的node

### git生成sshkey

通过ssh-keygen 命令生成公钥，如果使用默认的设置，会在~/.ssh目录下找到 id\_rsa.pub，通过vim打开，键入v，通过键盘选择公钥，在命令行中输入“+y，将选中内容复制到系统粘贴板当中，将公钥保存到对应的gitlab账号当中。

### 安装NERDTree插件

NERDTree插件的开源地址为[https://github.com/scrooloose/nerdtree](https://github.com/scrooloose/nerdtree)， 通过

```bash
wget http://www.vim.org/scripts/download_script.php?src_id=17123 -O nerdtree.zip
```

命令下载插件包并解压

```bash
unzip nerdtree.zip
```

下面将插件进行安装

```bash
mkdir -p ~/.vim/{plugin,doc}
cp plugin/NERD_tree.vim ~/.vim/plugin/
cp doc/NERD_tree.txt ~/.vim/doc/
```

在进入到目录后，运行vim 在命令模式下输入:NERDTree，你可以看到项目的结构目录树

接下来需要知道的就是一些常见的命令：

```
ctrl + w + h    光标 focus 左侧树形目录
ctrl + w + l    光标 focus 右侧文件显示窗口
ctrl + w + w    光标自动在左右侧窗口切换
ctrl + w + r    移动当前窗口的布局位置
o       在已有窗口中打开文件、目录或书签，并跳到该窗口
go      在已有窗口 中打开文件、目录或书签，但不跳到该窗口
t       在新 Tab 中打开选中文件/书签，并跳到新 Tab
T       在新 Tab 中打开选中文件/书签，但不跳到新 Tab
i       split 一个新窗口打开选中文件，并跳到该窗口
gi      split 一个新窗口打开选中文件，但不跳到该窗口
s       vsplit 一个新窗口打开选中文件，并跳到该窗口
gs      vsplit 一个新 窗口打开选中文件，但不跳到该窗口
!       执行当前文件
O       递归打开选中 结点下的所有目录
x       合拢选中结点的父目录
X       递归 合拢选中结点下的所有目录
e       Edit the current dif
双击    相当于 NERDTree-o
中键    对文件相当于 NERDTree-i，对目录相当于 NERDTree-e
D       删除当前书签
P       跳到根结点
p       跳到父结点
K       跳到当前目录下同级的第一个结点
J       跳到当前目录下同级的最后一个结点
k       跳到当前目录下同级的前一个结点
j       跳到当前目录下同级的后一个结点
C       将选中目录或选中文件的父目录设为根结点
u       将当前根结点的父目录设为根目录，并变成合拢原根结点
U       将当前根结点的父目录设为根目录，但保持展开原根结点
r       递归刷新选中目录
R       递归刷新根结点
m       显示文件系统菜单
cd      将 CWD 设为选中目录
I       切换是否显示隐藏文件
f       切换是否使用文件过滤器
F       切换是否显示文件
B       切换是否显示书签
q       关闭 NerdTree 窗口
?       切换是否显示 Quick Help
```

一些常见的处理tab页的命令：

```
:tabnew [++opt选项] ［＋cmd］ 文件      建立对指定文件新的tab
:tabc   关闭当前的 tab
:tabo   关闭所有其他的 tab
:tabs   查看所有打开的 tab
:tabp   前一个 tab
:tabn   后一个 tab
```



