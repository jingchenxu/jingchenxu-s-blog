## React 开发注意事项

#### 组件引入注意

#### 页面元素循环遍历注意

#### onscroll时间监听注意

#### 组件完全强制刷新方法

#### react页面强制刷新方法

#### 数据多层嵌套时的注意

#### 前端高精度计算问题

#### 子组件向父组件通信传值

#### 延时查询

#### 公共调用组件

#### 判断浏览器

```javascript
judgeClient: function () {
		let u = navigator.userAgent,
			app = navigator.appVersion;
		return {
			trident: u.indexOf('Trident') > -1, //IE内核
			presto: u.indexOf('Presto') > -1, //opera内核
			webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
			gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
			mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
			ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
			android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
			iPhone: u.indexOf('iPhone') > -1 , //是否为iPhone或者QQHD浏览器
			iPad: u.indexOf('iPad') > -1, //是否iPad
			webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
		}
	}
```