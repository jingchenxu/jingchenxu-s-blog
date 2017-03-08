## webpack跨域调试

### 前后端分离可能导致的问题

目前经手了2个微信项目，采用的都是前后端分离的开发模式，前端开发和后端的开发的分离从概念上是美好的，但是在实际的开发中会遇到很多的问题，尤其是微信开发，当然在微信开发中也有很多的坑，今天暂且不讲微信开发的坑。

2个微信项目都是网上商城，采用的开发模式为，前端采用的是react，后端采用的是springMVC框架，前后端分离本应成为项目有点的地方，在实际的操作中可能会存在很多的问题。

*采用json文件模拟接口*

第一个项目我就是采用的json文件来模拟接口进行开发，原本我的开发计划为我将我的json文件作为后端接口开发的标准，但是出现了问题，后端甩给我的接口与我的json文件区别非常的大，接口提供的数据，完全是整张表进行查询，而且返回数据的键值与数据库中的字段完全的一致，没有做任何的混淆。

我自顾自的按照自己设计的json文件进行开发，但是在最后才发现，自己模拟的接口与实际得接口的区别非常的大，所以要做好后端的沟通。

后端在接口的开发过程中，很多时候后端开发并不会对接口进行完善的测试，所以最好向后端开发要一份接口数据格式的文档，前台在组装数据的时候，就不需要一次次的去猜了。

*采用mock模拟数据*

第二个项目在开发一开始我做了一定的准备，第一个项目是我从0开始构建的，没有组件，没有redux，没有webpack（用的gulp进行打包），第二个项目鸟枪换炮，采用的dva框架下的atool-build+dora模式，在该模式下我的开发目前是完全基于后端的，我将后端接口所需的数据，用mock完全进行模拟，一开始感觉开发的很溜，但是在一次提交接口的对接中发现了问题，如果用mock做接口模拟，虽然数据可以模拟的很像，但是存在一个问题就是数据在提交的时候，与真实的接口提交的区别还是很大的。

### 采用webpack跨域调试工具

附上代码：
```javascript
    devServer: {
		historyApiFallback: true,
      	hot: true,
		inline: true,
		stats: { colors: true },
		proxy: {
			///list/column  --> http://10.2.130.20:9000/column
	        '/lflweb/prd/shprdtype/': {
	          target: 'http://192.168.100.100:8080',
	          pathRewrite: {'^/{}' : '/{}'},
	          changeOrigin: true
	        },
	        ///ok/ok  --> http://pod.gf.com.cn/api/information/podcastserver/1.0.0/episodes/category/57959fd6b05063000b284f58?page_no=1&page_size=1
	        '/ok':{
	        	target: 'http://pod.gf.com.cn/api/information/podcastserver/1.0.0',
	        	pathRewrite: {'^/ok':'/episodes/category/57959fd6b05063000b284f58?page_no=1&page_size=10'},
	        	changeOrigin: true
	        },

	        //	/categories -->http://pod.gf.com.cn/api/information/podcastserver/1.0.0/categories
	        '/categories': {
	        	target: 'http://pod.gf.com.cn/api/information/podcastserver/1.0.0',
	        	pathRewrite: {'^/': '/'},
	        	changeOrigin: true
	        }
	     }
	},
```

ajax请求的代码如下：

```javascript
$.ajax({
			type: "GET",
			url: "/lflweb/prd/shprdtype/{}",
			dataType: "json",
			xhrFields: {
				withCredentials: true
			},
			crossDomain: true,
			success: function (data) {
				console.log(data);
			}
		});
```