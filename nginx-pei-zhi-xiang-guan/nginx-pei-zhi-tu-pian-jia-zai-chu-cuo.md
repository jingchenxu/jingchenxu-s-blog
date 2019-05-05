# nginx 配置图片加载出错

* 遇到问题

最近刚刚测试部署自己的博客，突然发现首页的图片加载失败了，体积小点的图片加载有时会失败，体积大的图片是必然会失败，我看了图片请求的状态码，发现状态码是200，这是请求成功啊，然后看了一下浏览器调试工具中的console,发现果然时报错了，报错如下：`net::ERR_SPDY_PROTOCOL_ERROR`，想也不想的百度，我靠一堆说了等于没说还是复制粘贴的回答，怎么办，自己找喽。

* 解决

想到网络请求的状态码是200，那么在nginx的日志中是如何保存的呢，于是我在nginx中设置了一下错误日志的输出，果然在日志中看到了错误输出，输出如下：

```text
2018/01/24 19:16:45 [crit] 26038#0: *3284 open() "/usr/local/nginx/proxy_temp/4/03/0000000034" failed (13: Permission denied) while reading upstream, client: 114.221.125.29, server: jingchenxu.xin, request: "GET /daisy/img/spring.jpg HTTP/2.0", upstream: "http://127.0.0.1:4000/daisy/img/spring.jpg", host: "jingchenxu.xin", referrer: "https://jingchenxu.xin/daisy/"
```

好开心，仔细看错误，打开文件失败，没有权限，于是跑到该目录下设置权限，重试一下，果然好了。

