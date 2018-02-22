## nginx 配置history路由

- 为什么要使用history路由

我为什么要使用history路由，最直接的理由就是[单页应用中微信授权页面遇到的坑](/spa中微信支付授权目录设置.md),在遇到这样的问题后，我通过一些非常规的方法解决了此问题，但是我相信一定有一个常规的方法可解决此问题，那就是使用history路由，使用history路由后，地址链接中将不会再出现可恶的````#````,一切又会变得那么美好，但是美好都是有代价的。

- 如何使用history路由进行开发

如果使用history路由，是需要做一些开发配置的，在本地进行开发时需要配置webpack的配置，在部署到了生产环境之后，需要对nginx进行配置，在开发模式下对于webpack的配置如下：

````js
    devServer: {
        historyApiFallback: true,
        noInfo: false,
        overlay: true,
        proxy: {
            '/daisy/*': {
                target: 'http://localhost:4000',
                secure: false, //接受 运行在 https 上的服务
                changeOrigin: true
            }
        }
    },
````