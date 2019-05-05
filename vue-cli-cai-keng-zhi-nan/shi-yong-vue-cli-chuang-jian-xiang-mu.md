# 使用vue-cli创建项目

* 创建项目

使用vue-cli创建项目，首先需要全局安装vue-cli:

```text
npm install -g vue-cli
```

安装完成后如何初始化一个项目，可以使用vue-cli的帮助命令看看：

```text
$ vue -h

  Usage: vue <command> [options]


  Commands:

    init        generate a new project from a template
    list        list available official templates
    build       prototype a new project
    help [cmd]  display help for [cmd]

  Options:

    -h, --help     output usage information
    -V, --version  output the version number
```

我们可以看到list命令，该命令可以列出所有的vue-cli可创建的模板项目，接下来我们看看可创建哪些模板项目：

```text
$ vue list

  Available official templates:

  ★  browserify - A full-featured Browserify + vueify setup with hot-reload, linting & unit testing.
  ★  browserify-simple - A simple Browserify + vueify setup for quick prototyping.
  ★  pwa - PWA template for vue-cli based on the webpack template
  ★  simple - The simplest possible Vue setup in a single HTML file
  ★  webpack - A full-featured Webpack + vue-loader setup with hot reload, linting, testing & css extraction.
  ★  webpack-simple - A simple Webpack + vue-loader setup for quick prototyping.
```

这时候可以看到可以创建的模板项目有这么多，你可以创建一个包含全部特性的项目。

```text
vue init webpack
```

