## vs-code 使用记录

> 个人据地vs-code是win平台下个人觉得比较好的一个文本编辑器，所以打算记录下我在其使用过程中的一些心得，[vs-code地址](https://code.visualstudio.com/)，使用的原因主要是颜值好，功能好（插件多），原生支持markdown预览。

> 吐槽一下：其实到目前为止没什么可吐槽的，vscode其实很不错，但是请注意下载的时候下载stable版本的，改过下载insider版本的话，其会频繁的更新，当然也会导致很多的BUG，但是到github的issue中看一下，就知道每天的问题数，想想也就释然了，建议使用稳定版本。

### 简介

选择vscode，其实还有一个比较重要的原因，因为其的多平台支持，我最终还是梦想拥有一个mac笔记本电脑。

### 基本设置

vscode配置目录为：文件->首选项->设置->settings.json 
编辑器的左侧会显示所有的可配置选项，在右侧则编辑对应的选项，具体编辑的格式可如下：

````json
// 将设置放入此文件中以覆盖默认设置
{
  //以像素为单位控制字号。
  "editor.fontSize": 18,
  //文档自动保存
  "files.autoSave": "afterDelay",
  //配置express的默认端口号为2014
  "express.portNumber": 2014,
  //显示文件图标
  "workbench.iconTheme": "vscode-icons",
  //react代码格式化
  "react.beautify.onSave": true,
  //编辑器自动换行
  "editor.wordWrap": "on",
  "vsicons.projectDetection.autoReload": true
}
````

以上看起来配置还是很方便的，vscode官方支持中文汉化，每个配置项的上面都会有配置注解，看起来很方便。

### 常用插件
- [Express](https://marketplace.visualstudio.com/items?itemName=Compulim.vscode-express)(静态文件服务器，在开发静态页的时候比较有用。)
- [HTML Snippets](https://marketplace.visualstudio.com/items?itemName=abusaidm.html-snippets)(写html是会有代码提示)
- [file-icons](https://marketplace.visualstudio.com/items?itemName=file-icons.file-icons)(会根据不同的后缀名显示不同的文件图标)
- [vscode-refresh-html](https://marketplace.visualstudio.com/items?itemName=bajdzis.vscode-refresh-html)(静态页自动刷新)

### 快捷键介绍

ctrl+shift+o:查看当前文件中所有的方法和对象；
alt+shift+a:取消或添加代码块注释；


### 参考文档

【1】[【VSCode】配置完全手册（编写中）](http://blog.csdn.net/greatbody/article/details/54581226)<br/>



