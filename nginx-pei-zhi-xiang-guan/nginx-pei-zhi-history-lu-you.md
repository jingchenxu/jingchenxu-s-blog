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

在使用vue-router时路由的配置如下：

````js
// 路由配置
const RouterConfig = {
    mode: 'history',
    //base: '/daisyadmin',
    routes: routers
};
````

需要注意的是，如果一旦你在路由配置中添加了路由的根目录后，会导致一个问题，那就是目前的devserver配置在手动刷新浏览器后页面会出现404，具体的解决此问题的方法目前还没有找到。

在开发模式下，所有的请求都会被webpack配置的devServer代理，通过代理，会将所有的404都转发到配置的html页面，那么在生产环境下，这些工作需要交由nginx来完成，关于nginx的配置可以参考如下的配置：

````
    server {
        listen       80;
        listen       443 ssl http2; // http强制跳转到https
        #        ssl          on;
        ssl_certificate   /etc/nginx/ssl/jingchenxu.xin/214433178320428.pem;
        ssl_certificate_key  /etc/nginx/ssl/jingchenxu.xin/214433178320428.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers AESGCM:ALL:!DH:!EXPORT:!RC4:+HIGH:!MEDIUM:!LOW:!aNULL:!eNULL;
        ssl_prefer_server_ciphers on;

        server_name  www.jingchenxu.xin;

        #charset koi8-r;

        access_log  /jingchenxu/log/www/daisy.access.log;
        error_log   /jingchenxu/log/www/daisy.error.log;

        #lottery
        location / {
           proxy_set_header x-forwarded-for $remote_addr;
           proxy_pass http://www.jingchenxu.xin;
           rewrite "^/+$" /daisy/ redirect;
           root /daisy/;
        }

        # 专门用于处理前端资源
        location /daisyadmin{

           root   /data/nginx/html;
           index  index.html index.htm;

           # 解决使用history路由的问题
           if (!-e $request_filename) {
               rewrite ^/(.*) /daisy/admin/index.html last;
               break;
           }
        }
    }

````