## javascript中闭包

closure 百度了一下，闭包的英文是这样的，至于中文为什么翻译为闭包，不是很了解。

- 是什么驱动我来了解闭包

这篇文章在之前就已经写过了，但是最近我把他给删除了，因为可能我之前的理解有一些问题，导致我在遇到接下来的问题的时候，没有头绪，但是最后误打误撞的又弄对了。

首先说一下问题，vue当中提供了动态路由的功能，这对于解决前端页面权限控制提供了一个实现思路，可以再用户登录之后，从后台接口获取该用户可访问的菜单列表，接口中获取的是菜单的配置参数，我们需要转化为vue路由，具体的代码可以参考下面的代码：

````javascript
		for(var j=0; j<RouterList.length; j++) {
			var temp = RouterList[j];
			RouterList[j] = function(){
				return {
					path: temp.path, name: temp.name, component: function(resolve) {
						console.log('js地址为'+temp.jsUrl);
						return require([temp.jsUrl], resolve);
					}
				}
			}();
		}
````

运行之后会发现，path、name的值都是正确的，但是在点击菜单切换路由的时候发现，加载的组件都是同一个，通过代码中的日志可以发现，请求的url都是同一个url，这是为什么呢？我想到之前看的某篇博客，好像就是这样的问题，先随便改改，看能不能随便改好。

我感觉可能是传入的参数有问题，于是我单独的创建一个函数来处理配置文件的转化：

````javascript
		function transe(temp){
			return {
				path: temp.path, name: temp.name, component: function(resolve) {
					console.log('js地址为'+temp.jsUrl);
					return require([temp.jsUrl], resolve);
				}
			}
		}
		for(var j=0; j<RouterList.length; j++) {
			var temp = RouterList[j];
			RouterList[j] = transe(temp);
		}
````

这样修改后发现问题莫名的就好了，但是其实我们完全可以不这样做，在理解完闭包后我是这么做的：

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

这段代码与第一段代码的区别在哪里呢？这里先说一下，返回对象中的函数绑定的作用域发生了变化，接下来我们开始探讨js闭包。

- 还原问题

还原问题时间比较蛋疼的事情，你需要提炼问题出现的场景，这样你才能复现，某些问题只有特定的情况才会出现，那上面的问题什么时候会出现呢？我后来发现上面代码中component组件的加载采用的是惰加载，也就是说，component对应的方法是在你需要的时候才会执行的，而path、name的赋值在立即执行函数执行的时候，就已经完成了赋值。

根据以上信息，我复原出了这样的场景：

````javascript
        var testArray = [{ order: 1, event: '' }, { order: 2, event: '' }, { order: 3, event: '' }, { order: 4, event: '' }, { order: 5, event: '' }, { order: 6, event: '' }];

        function bindE(i) {
            return function() {
                console.log('show aside' + i);
            }
        }

        for (var i = 0; i < testArray.length; i++) {

                // planA
                testArray[i].event = function(i) {
                    console.log('立即执行函数内部'+i);
                    return function() {
                        console.log('show closure' + i);
                    }
                }(i)
                
                // planB
/*                 testArray[i].event = function () {
                    console.dir(this);
                    console.log('show:' + i);
                } */

                if(i !=0 ) {
                    console.log('====================');
                    console.dir(testArray[i-1].event);
                }

                // planC
/*                 testArray[i].event = bindE(i); */

        }

        console.dir(testArray[1].event);// target1
        console.dir(testArray[3].event);// target3
        console.dir(testArray[2].event);
        

        testArray[1].event();
        testArray[3].event();
        testArray[2].event();
````

planA与planB没有本质的区别，都是通过向立即执行函数中传入需要绑定至返回函数命名空间的参数，获取planC可以更清楚的看出，函数在声明时会将函数当前所在函数的作用域作为自己的命名空间，然后通过作用域链，获取父函数的作用域。

我们分别在planA与planB的情况下查看 target1与target2：

![planA](/img/javascript/planA.png)

![planB](/img/javascript/planB.png)

我们可以看到planA与planB的作用域是不同的，在planB中，我们可以看到i与counter在同一个作用域，i在for循环当中，最后被赋值为6，故而event对应的方法其作用域中的i的值为6。

综上，闭包是为了给函数创建一个只有该函数可访问的作用域，而之前的作用域都是通过函数嵌套创建的(作用域链)，而闭包则实现了在函数嵌套方法外添加函数作用域的方法。闭包可以实现在通过函数嵌套的方式外创建作用域。

- 是时候说说闭包了

百度一下，关于函数闭包的说法，大都是能够访问函数内部变量，由于js特有的作用域链，嵌套函数的内部函数可访问父函数的作用域，但是父函数无法访问子函数的作用域，那么想一下，是不是说明子函数是私有的，在java bean中如何访问私有属性，通过get\set,而闭包也是通过暴露特权方法来访问子函数的私有变量。

写到现在，或许闭包的存在是作为javascript 权限控制关键字缺失的一种补救方案，应为java就没有闭包啊！



