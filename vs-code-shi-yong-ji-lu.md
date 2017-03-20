## vs-code 使用记录

> 个人据地vs-code是win平台下个人觉得比较好的一个文本编辑器，所以打算记录下我在其使用过程中的一些心得，[vs-code地址](https://code.visualstudio.com/)，使用的原因主要是颜值好，功能好（插件多）。

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

### 常用插件

