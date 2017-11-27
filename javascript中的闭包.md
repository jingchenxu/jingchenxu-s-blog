## javascript中闭包

closure 百度了一下，闭包的英文是这样的，至于中文为什么翻译为闭包，不是很了解。

- 是什么驱动我来了解闭包

这篇文章在之前就已经写过了，但是最近我把他给删除了，因为可能我之前的理解有一些问题，导致我在遇到接下来的问题的时候，没有头绪，但是最后误打误撞的又弄对了。

首先说一下问题，vue当中提供了动态路由的功能，这对于解决前端页面权限控制提供了一个实现思路，可以再用户登录之后，从后台接口获取该用户可访问的菜单列表，接口中获取的是菜单的配置参数，我们需要转化为vue路由，具体的代码可以参考下面的代码：

````javascript
		for(var j=0; j<RouterList.length; j++) {
			var temp = RouterList[j];
			RouterList[j] = function(temp){
				return {
					path: temp.path, name: temp.name, component: function(resolve) {
						console.log('js地址为'+temp.jsUrl);
						return require([temp.jsUrl], resolve);
					}
				}
			}(temp);
		}
````