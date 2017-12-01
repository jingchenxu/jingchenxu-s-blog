## vue + requirejs + ES5 的后台管理系统

- 前言

为什么需要一个脱离前端构建工具的项目，在很多的后台管理系统当中，对于页面的要求可能不是那么的强烈，后台管理系统要求最多的在于增删查改，页面最多的在于列表与详情，对于小型项目来说，如何最快的画出页面，并与后端对接好事放在首位的需求。

前端的繁荣发展，更多的是因为应用的互联化话，产品的受众人群范围更广，类型更多，不在是管理系统对应的特定的人群，所以对于页面的要求也会更多，但是对于后台管理系统，很多时候都是作为后端的附属存在的，在2008年左右的时候，Ext作为一种面向对象的框架，在后台管理系统中占据了很重要的位置，后端人员可以轻易的掌握，快速的应用。

采用ES5主要是因为，可以直接在浏览器上跑，不需要编译，没有工程化，那么项目相对来说也会入手比较简单，随着前端工程化后，个人感觉前端的门槛又变高了，和java一样，搞好环境都要花上半天，所不定一个星期，而且你一个代码都还没写，只为搭建一个环境。

- Vue

项目虽然没有工程化，但是还是会使用到vue的全家桶：Vue + Vue-router + Vuex,我们会使用requirejs加载编译后的版本，而不是使用node_modules,你可以使用CDN获取这些js标签，可以使用一下的方式引入到html当中，当然你可以选用相对来说比较新的版本：

````html
<script type="text/javascript" src="https://cdn.bootcss.com/require.js/2.3.5/require.min.js"></script>
<script type="text/javascript" src="require.config.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vuex/dist/vuex.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
````

以上引入的js为项目基础的js，但是我们使用了requirejs,所以在html当中只需要引入requirejs就可以了，其他的js文件可以通过requirejs来加载，接下来我们完成一个最基本的配置。

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>require</title>
</head>
<body>
<div id="app"></div>
</body>
<script type="text/javascript" src="https://cdn.bootcss.com/require.js/2.3.5/require.min.js"></script>
<script type="text/javascript" src="require.config.js"></script>
<script type="text/javascript">
    // require 不会阻塞页面的渲染 页面入口
    require(["Vue", "routes", "VueRouter", "App", "iview"],function (Vue, routes, VueRouter, App, iview) {
    	var router = new VueRouter({
            routes: routes
        });
    	Vue.use(VueRouter);
       Vue.use(iview);
    	var app = new Vue({
            el: "#app",
            router: router,
            render: function (h) {
                return h(App);
            },
            created: function () {
               console.log('首页组件加载完成',routes, App);
            }
        })
    });
</script>
</html>
````

需要注意的是这里的vue-router需要全局注册，不然在使用router-view时会提示组件未注册的问题。

我们看一下requirejs的配置文件：

````javascript
require.config({
	paths: {
		"jquery": ["https://cdn.bootcss.com/jquery/3.2.1/jquery"],
		"Vue": ["js/vue"],
		"Vuex": ["js/vuex"],
		"VueRouter": ["js/vue-router"],
		"Vtest1": ["require/Vtest1"],
		"Vtest2": ["require/Vtest2"],
		"Vtest3": ["require/Vtest3"],
		"routes": ["router/router"],
		"App": ["require/app"],
		"Config": ["config/SeedConfig"],
		"Utils": ["config/SeedUtils"],
		"Axios": ["js/axios.min"]
	}
});
````

在此文件中，我们可以配置各个js文件对应的名称，相当于为每个js起了一个别名，我们可以在require和define中通过此别名来获取对应的js作为依赖。

因为所有的js是通过requirejs来管理的，所以我们书写的js需要符合requirejs的规范，下面我们创建一个Vue组件模块：

````javascript
define(["Vue"], function (Vue) {
	var Vtest1;
	Vtest1 = Vue.extend({
		name: 'Vtest1',
		template: '<div>Vtest1<router-view></router-view></div>',
		data: function () {
			return {

			};
		},
		created: function () {
			alert('Vtest1组件创建完成');
		}
	});

	return Vtest1;
});
````

我们可以看到，可以在define的第一个参数中申明当前组件的依赖，申明后则可以在组件模块内使用，以上我们基本完成了一个项目的骨架。

- UI库集成（iview）

项目使用的是iview组件库，但是需要注意的是，如果使用requirejs,无法直接使用其官网提供的的unpkg上提供的CDN链接，因为其实按照UMD标准打包编译的，需要修改编译标准，修改为AMD,此外编译打包后的文件中会发现其依赖的为Vue为["vue"],其首字母为小写，如果你在require.config中paths配置的vue的首字母为大写，则可能会导致iview模块加载其vue依赖失败。

修改iview打包最后模块规范可以在其的webpack配置文件中修改，可以分别在build目录下的webpack.dist.dev.config.js、webpack.dist.locale.config.js、webpack.dist.prod.config.js中修改libraryTarget参数中修改为amd,然后通过Vue.use(iview)全局注册iview组件。






