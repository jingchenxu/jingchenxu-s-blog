## react全家桶历险记

### 初始化项目

首先我显示在github上建立了一个项目，鉴于我在此前已经建立过无数的项目的，每次都希望能够好好学习，但是最后都是不了了之。

这次是为了系统的学习react、react-router、redux、webpack、bootstrap这样几个框架而准备的。

在项目``npm init``项目初始化完成之后，我遇到了一个问题，那就是在安装react包的时候出现的一下的报错，我执行的语句为``npm install react --save``，出现的错误如下：

```shell

Microsoft Windows [版本 6.1.7600]
版权所有 (c) 2009 Microsoft Corporation。保留所有权利。

E:\Users\jinhetech.com>g:

G:\>cd nestle

G:\nestle>cd react-reactRouter-redux-bootstrap

G:\nestle\react-reactRouter-redux-bootstrap>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (react-reactRouter-redux-bootstrap)
Sorry, name can no longer contain capital letters.
name: (react-reactRouter-redux-bootstrap) react
version: (1.0.0)
description: this is a demo for react-reactRouter-redux-ES6-webpack
entry point: (index.js)
test command: jingchenxu
git repository: (https://github.com/jingchenxu/react-reactRouter-redux-bootstrap
.git)
keywords: react|react-router|redux|ES6|webpack
author: jingchenxu
license: (ISC)
About to write to G:\nestle\react-reactRouter-redux-bootstrap\package.json:

{
  "name": "react",
  "version": "1.0.0",
  "description": "this is a demo for react-reactRouter-redux-ES6-webpack",
  "main": "index.js",
  "scripts": {
    "test": "jingchenxu"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/jingchenxu/react-reactRouter-redux-bootstrap.
git"
  },
  "keywords": [
    "react|react-router|redux|ES6|webpack"
  ],
  "author": "jingchenxu",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/jingchenxu/react-reactRouter-redux-bootstrap/issu
es"
  },
  "homepage": "https://github.com/jingchenxu/react-reactRouter-redux-bootstrap#r
eadme"
}


Is this ok? (yes) yes

G:\nestle\react-reactRouter-redux-bootstrap>npm install react --save
npm ERR! Windows_NT 6.1.7600
npm ERR! argv "E:\\Program Files\\nodejs\\node.exe" "E:\\Users\\jinhetech.com\\A
ppData\\Roaming\\npm\\node_modules\\npm\\bin\\npm-cli.js" "install" "react" "--s
ave"
npm ERR! node v4.4.4
npm ERR! npm  v4.0.1
npm ERR! code ENOSELF

npm ERR! Refusing to install react as a dependency of itself
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR!     <https://github.com/npm/npm/issues>

npm ERR! Please include the following file with any support request:
npm ERR!     G:\nestle\react-reactRouter-redux-bootstrap\npm-debug.log

G:\nestle\react-reactRouter-redux-bootstrap>

```

以上就是我初始化的过程，面对这样的问题，百度出的解决方案为：[百度结果](https://segmentfault.com/q/1010000000164925)，问题在于我初始化的项目的名称叫做``react``，而此时安装的包的名称恰好也叫react，所以就命名冲突了。

### 如何创建一个git忽略文件

一些项目必备的npm包安装完成之后，项目的体积会变得很大，这个时候为了保证我们在上传文件的时候，项目的体积不要太大，我们可以把一些完全没有必要的文件夹进行忽略``touch .gitignore``,但是该命令必须在``git bash``中执行。

创建完成之后，可以通过编辑器直接打开该文件夹，我添加了``/node_modules/``,则项目的代码在上传 时候则会自动的忽略该文件夹。

