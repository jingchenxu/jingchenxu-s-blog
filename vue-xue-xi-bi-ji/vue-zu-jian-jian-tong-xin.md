## vue 组件间通信

- 组件通信的机制

不仅是在vue组件之间存在组件间通信，在react组件中也存在组件间的通信，如果在深入一点会发现，安卓与ios的模块间页存在通信的问题，而且同行的方式方式都很类似，父组件向自组件的通信通过类似于prop的方式传递，子组件向父组件则采用类似回调的方式来通知父组件，对于的平行组件之间的通信，我们会使用vuex这样的前端数据持久化层来处理，react可以使用redux、mobx,在早起的react项目中，我会使用使用一个事件注册中心来处理组件之间的通信，而vue也提供了一个很方便的方式来处理时间注册与时间监听，下面的代码展示了Vue组件间的各种通信方式：

````javascript
define(["Vue"],function (Vue) {

	var son = Vue.extend({
		name: 'son',
		props: ['callparent', 'buttonText'],
		render: function(h){
			var me = this;
			return h('Button', {
				domProps: {
					innerHTML: me.buttonText
				},
				on: {
					click: function(e) {
						me.callparent();
						eventBus.$emit('msg', 12);
						e.cancelBubble = true;
					}
				}
			});
		}
	});

	

	var CallParent = Vue.extend({
		name: 'CallParent',
		components: {
			'son': son
		},
		data: function() {
			return {
				text: 'TI JIAO'
			}
		},
		render: function (h) {
			var me = this;
			return h('div', {
				style: {
					width: '300px',
					height: '300px',
					backgroundColor: 'red'
				},
				on: {
					click: function() {
						me.text = "父组件告知子组件"	
					}
				}
			},[h('son', {
				props: {
					callparent: this.call,
					buttonText: me.text
				}
			})]);
		},
		created: function() {
			eventBus.$on('msg', function(){
				alert('通过eventBus获取到消息');
			})
		},
		methods: {
			call: function() {
				alert('father get');
			}
		}
	});

	return CallParent;
});
````

原谅我任性的使用ES5。