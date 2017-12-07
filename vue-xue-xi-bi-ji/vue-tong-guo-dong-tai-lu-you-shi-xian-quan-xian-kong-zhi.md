## vue 通过动态路由实现权限控制

- 权限控制

对于权限控制之类的问题，如果采用的是传统的动态页面技术（jsp,.net）进行开发的时候,我们完全可以在后端完成权限的控制，但是如果是使用的SPA技术开发出的前端页面的话，可能会有一些问题，因为前端的路由都是写死的，理论上来说，我们将所有的前端页面都加载到了客户端，我们可以在前端的js当中处理页面授权问题，但是因为路由已经完全暴露，可能会有一些风险。

- vue-router通过动态路由实现权限控制

进入到用户业务界面，通过向后台进行ajax请求获取到用户的权限信息，这里通过json文件进行用户菜单的模拟，json文件如下：

````json
[
    {
        "groupName": "basic demo",
        "icon": "ios-navigate",
        "order": "1",
        "children": [
            {
                "path": "/moredetail",                 
                "jsUrl": "views/seeddemo/Detail",     
                "order": "moredetail",
                "name": "moredetail"
            },
            {
                "path": "/moredetail1",                
                "jsUrl": "views/seeddemo/ListDemo",      
                "order": "moredetail1",
                "name": "moredetail1"
            },
            {
                "path": "/formdemo",                
                "jsUrl": "views/seeddemo/FormDemo",      
                "order": "formdemo",
                "name": "formdemo"
            }
        ]
    },
    {
        "groupName": "item2",
        "icon": "ios-keypad",
        "order": "2",
        "children": [
            {
                "path": "/moredetail3",                 
                "jsUrl": "views/seeddemo/Detail",      
                "order": "moredetail3",
                "name": "moredetail3"
            },
            {
                "path": "/moredetail4",                 
                "jsUrl": "views/seeddemo/Detail",      
                "order": "moredetail4",
                "name": "moredetail4"
            }
        ]
    },
    {
        "groupName": "item3",
        "icon": "ios-analytics",
        "order": "3",
        "children": [
            {
                "path": "/moredetail5",                
                "jsUrl": "views/seeddemo/Detail",      
                "order": "moredetail5",
                "name": "moredetail5"
            }
        ]
    }
]
````

用户从登陆页到业务操作页面时，在页面加载完成后通过用户的登录信息获取其对应的菜单：

````javascript
		mounted: function() {
			var me = this;
			// 判断缓存中是否有用户数据
			this.$axios_get('seedjson/menulist.json').then(function(data){
				console.dir(data);
				me.menuList = data.data;
				// 将路由数据添加到缓存当中
				jscookie.set("routes", data.data);

			    /**
				 * @author jcxu
				 * @description 用户在从登陆页面跳转到此页面时 从服务端获取相应的菜单信息，并动态的加载，需要注意的是，如果用户进行了全局刷新，那么久需要从服务端重新获取相应的菜单信息 
				 */
				me.$router.addRoutes([{
					path: '/layout1',
					name: 'layout1',
					component: function(resolve) {
						return require(["TemplateLayout"], resolve);
					},
					children: SeedUtils.changeMenuToRouter(data.data)
				}])

			})
		}
````

使用动态路由会遇到这样的问题，用户在刷新浏览器的时候会丢失动态加载的路由，这就需要我们在路由信息加载完成的时候将路由信息缓存到客户端，当用户刷新的时候，先检查客户端是否存在路由信息，如果存在则加载客户端的路由信息：

````javascript
            beforeCreate: function () {
                var me = this;
                //看看cookie中是否有值，有的话就动态添加路由
                //console.dir(jscookie.get("routes"));
                var routes = jscookie.get("routes");
                // TODO 还需要判断用户是不是处于登录的状态
                if (routes) {
                    me.$router.addRoutes([{
                        path: '/layout1',
                        name: 'layout1',
                        component: function (resolve) {
                            return require(["TemplateLayout"], resolve);
                        },
                        children: SeedUtils.changeMenuToRouter(eval(routes))
                    }])
                }
            },
````

以上大致可以实现通过动态路由进行权限控制，还存在一些刷新时的细节要调整下。
