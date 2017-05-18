## tomcat 开启gzip

>客户最近反馈项目的打开速度比较慢，项目是一个微信商城的项目，采用SPA的方式进行开发的，算是webAPP模式，目前主js文件的体积达到了900kb左右，css的体积为100kb左右，这还是在按需加载的情况下，首屏的加载时间是长了点，当然我发现其实加载速度和手机也是有关系的，在苹果手机上的加载速度比较的快。

好了，客户就是上帝，客户既然提出了这个问题，那我们就要着手开始解决了，这篇文章算是SPA应用提升加载速度的一个教程吧！

其实在此之前我就已经意识到这个问题了，所以才做了按需加，按需加载只要在react-router中配置成这样就可以了：

```javascript
path: 'onlineMarket',
      name: 'onlineMarket',
      getComponent(nextState, cb) {
        require.ensure([], require => {
          cb(null, require('./page/wrapper/Wrapper'))
        })
      },
```

采用这种模式使用webpack打包的时候，webpack会将每个页面打包成一个函数，每个函数会被一个json进行管理，最后通过eval执行这个函数，从而实现按需加载，需要使用的时候才会向服务器请求，大概你能看到的就是这个样子：

![](/img/gzip/gzip1.png)

其实这种方式只是分散了体积，用户在使用过程中，在遇到需要按需加载的模块，且该页面的js文件的体积比较大的时候，需要先去请求js文件，在执行函数渲染页面，如果网络不好或是一般的话，用户在首次使用的时候，会出现页面的卡顿，使用的效果也不是很好，但是这一步最起码快了很多，在项目较小的时候还是能接受的。

项目进入后期，js的体积不断的变大，首屏的加载时间开始逐渐边长，这时候无意间看到gzip，是时候了，今天又被客户抨了一下。

在tomcat->conf->service.xml中找到 Connector 节点，修改你所在项目使用的端口号对应的节点：

```xml
<Connector URIEncoding="UTF-8" port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
	           compression="on"
	           compressionMinSize="50"
	           noCompressionUserAgents="gozilla, traviata"
	           compressableMimeType="text/html,text/xml,application/javascript,text/css,text/plain"/>
```
该配置的gzip对8080端口有效，compression="on" 开启gzip压缩，compressionMinSize="50" 对于体积低于50kb的文件不进行压缩， noCompressionUserAgents="gozilla, traviata" 对某些浏览器的请求不进行压缩，compressableMimeType="text/html,text/xml,application/javascript,text/css,text/plain" 主要是设置对html、js、css文件进行压缩，注意很多的教程上javascript的mime类型写的是text，但是在tomcat7中javascript的mime类型为application。

以上修改完了之后，重启一下tomcat服务器，然后我们请求一个页面，观察一下，看看gzip是否有效：

![](/img/gzip/gzip2.png)

简直amazing，原本体积700多kb的压缩后仅有199kb,你还在等什么？









