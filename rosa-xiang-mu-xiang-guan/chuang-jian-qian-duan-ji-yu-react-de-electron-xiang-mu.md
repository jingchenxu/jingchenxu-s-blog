# 创建前端基于react的electron项目

> 尝试了较多的桌面应用开发方式后，最后发现可能electron是最好的选择，我就是想做一个根据数据库表结构生成代码的工具，这样的应用就是将一个web应用转化为一个桌面应用，之前试过WPF,javaFX,pythonwx,他们都不错，但是有2点不好看，我不熟,想要画出好看的GUI不容易。

## 创建一个create-react-app项目

create-react-app是个类似vue-cli的项目创建工具，比2年前好用多了，以后升级也方便多了，再也没有一步步从webpack搭建一个工程化项目的痛苦，但却有点怀念。

我们创建好一个create-react-app项目后，开始通过yarn添加electron依赖，添加完成后，设置后端进程的代码入口，这里设置为main.js,通过create-react-app创建的项目的前端入口文件应该是index.js,electron项目中一般会有2个进程，这2个进程通过rpc进行通讯。

[如何创建electron应用](https://electronjs.org/docs/tutorial/first-app)，按照这里面的提示可以创建，如果按照这样的形式创建的项目，那么渲染线程的入口文件是src文件夹下面的index.js文件，后端的主线程是根目录下的main.js,在package.json设置默认的入口文件，package.json文件的内容如下：

```javascript
{
  "name": "rosa",
  "version": "1.0.0",
  "description": "code generator base on database",
  "main": "main.js",
  "private": true,
  "scripts": {
    "start-app": "cross-env NODE_ENV=development electron .",
    "test-app": "echo \"Error: no test specified\" && exit 1",
    "build-css": "node-sass src/ -o src/",
    "watch-css": "npm run build-css && node-sass src/ -o src/ --watch",
    "start": "cross-env NODE_ENV=development react-scripts start",
    "build": "cross-env NODE_ENV=production react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "start-all": "npm-run-all -p start start-app"
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/jingchenxu/rosa.git"
  },
  "keywords": [
    "electron",
    "react"
  ],
  "author": "jingchenxu2015@gmail.com",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/jingchenxu/rosa/issues"
  },
  "homepage": "https://github.com/jingchenxu/rosa#readme",
  "devDependencies": {
    "cross-env": "^5.2.0",
    "electron": "^4.0.1",
    "node-sass": "^4.11.0",
    "npm-run-all": "^4.1.5"
  },
  "dependencies": {
    "mysql2": "^1.6.4",
    "prop-types": "^15.6.2",
    "react": "^16.7.0",
    "react-dom": "^16.7.0",
    "react-router-dom": "^4.3.1",
    "react-scripts": "^2.1.3"
  },
  "browserslist": [
    ">0.2%",
    "not dead",
    "not ie <= 11",
    "not op_mini all"
  ]
}
```

## 项目配置注意事项

* 跳过版本检查

这个问题我只在win10下遇到了，比较奇怪，给项目添加.env文件即可：

```text
SKIP_PREFLIGHT_CHECK=true
```

* 国内环境下依赖下载缓慢

公司电信的网络环境下还好，在住的地方的移动环境就有点问题了，electron这个依赖下载的速度十分的缓慢，将镜像地址注册为国内的地址还是不行，我觉得可能是electron的依赖指定了国外的下载地址，后来找到了一个好东西.npmrc，目前我主要使用yarn,想着那yarn应该也是有.yarnrc文件的，没有错，.yarnrc文件是存在的，.npmrc、.yarnrc文件配置如下，通过rc文件可以指定哪些依赖的下载到指定的下载源进行下载：

```text
registry=https://registry.npm.taobao.org/
disturl=https://npm.taobao.org/mirrors/node
ELECTRON_MIRROR=https://npm.taobao.org/mirrors/electron/
```

```text
registry "https://registry.npm.taobao.org"

sass_binary_site "https://npm.taobao.org/mirrors/node-sass/"
phantomjs_cdnurl "http://cnpmjs.org/downloads"
electron_mirror "https://npm.taobao.org/mirrors/electron/"
sqlite3_binary_host_mirror "https://foxgis.oss-cn-shanghai.aliyuncs.com/"
profiler_binary_host_mirror "https://npm.taobao.org/mirrors/node-inspector/"
chromedriver_cdnurl "https://cdn.npm.taobao.org/dist/chromedriver"
```

* 设置环境变量

在脚本启动命令中需要设置环境变量，但是直接设置好像有问题，需要安装cross-env依赖。

* 多个脚本同时运行

在electron项目中需要同时运行渲染线程及后端线程，可以安装start-all这个依赖。

