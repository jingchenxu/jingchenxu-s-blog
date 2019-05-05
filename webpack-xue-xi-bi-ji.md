# webpack学习笔记

前端要跟随时代的潮流，更要打好基础

ES6可能在生产环境中或是某些地方使用的不是很多，但是众多的github项目中开始全面的使用ES6语法，前端的打包工具也发生grunt=&gt;gulp=&gt;webpack这样的迁徙，所以打算将项目中的gulp修改为webpack，同时配置文件使用ES6的语法。

## step1

1.\`\`\` npm install babel-preset-es2015 --save-dev

```text
2.在```webpack.config.js```的同目录下创建文件```.babelrc```,内容如下

````json
{
  "presets":["es2015"]
}
`
```

## 常见问题

在webpack1.6.3后的版本中webpack如果将热加载的打包文件和webpack-dev-server.js的地址配置为ip地址的话，是会出现如下问题的：

\`\`\`“Invalid Host Header” in When running React App

```text
这是后需要修改webpack的配置文件：
```

devServer: { historyApiFallback: true, disableHostCheck: true, hot: true, inline: true, stats: { colors: true }, proxy: {

\`\`\`

