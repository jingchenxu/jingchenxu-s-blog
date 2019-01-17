# vue 后台管理类项目兼容IE

> 最近项目进入到了第三方集成的环节，对方非要用IE,咋办？老板让我将后台管理系统的框架兼容下IE.

## 目前后台管理系统前端搭建方式

目前系统是用vue-cli@2.0生成的，UI框架使用的是iview,ajax请求使用的是axois,此外就没有什么特殊的npm包了。

## 需要解决的兼容问题

1. 在IE下由于没有promise，所以axios不能用了；
2. 在涉及到flex,fixed,absolute定位时，IE浏览器的显示效果有较大的区别；
3. excel表单导出异常；
4. 部分使用的npm包中的代码未经编译会有一些ES6的语法；
4. vue-router路由失效。

## 如何解决这些问题

1. 解决第一个问题需要在项目中引入babel-polyfill， 我的处理方式时在build->webpack.base.config.js文件中的添加一下的配置：
![](/img/VUE/微信截图_20190117170159.png)
2. 解决第二个问题则需要自己写一些兼容性比较好的样式，在这里我就不做过多的解释了。
3. 第三个问题的解决过程比较的曲折，系统的下载是同过接口返回文件流的形式进行下载的，可以看下我原来的代码，原先通过axios的拦截器来获取响应内容的格式，然后进行下载，但是在IE的兼容测试过程中发现了一个问题，那就是axios在chrome和IE下的表现不一致，具体哪里一致可以简单的说下：
````javascript
  const downloadUrl = url => {
    let iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = url;
    iframe.onload = function () {
      document.body.removeChild(iframe);
    };
    document.body.appendChild(iframe);
  };
    // 拦截二进制响应流
    if (res.headers && (res.headers['content-type'] === 'application/vnd.ms-excel;charset=UTF-8' || res.headers['content-type'] === 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' || res.headers['content-type'] === 'application/octet-stream;charset=UTF-8')) {
      downloadUrl(res.request.responseURL);
      return
    }
````
这里面的问题在于在IE浏览器中res.request.responseURL这个属性是不存在的，就算存在了，在进行文件下载时也会出现异常，后台看到很多人都出现了这样的问题，怎么办，我相信这个问题一定是可以解决的，虽然没有搜到合适的方案，但是一个网友提示可以



