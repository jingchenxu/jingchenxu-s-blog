## Promise 什么鬼

- promise解决了什么

promise主要解决的问题是异步编程的问题，最常见的一个场景是我们在使用ajax请求的时候，我们需要对ajax请求的结果进行各种处理，这个时候通常我们会使用回调函数，有时候在ajax中还嵌套ajax的时候，我们会写出多层嵌套的代码，此时代码的可读性会比较差，通过promise我们可以写出代码结构类似同步的异步代码。

- 简单的看一下promise

通过上面的描述我们可以把promise看做是一个异步的管理器，设计的模式有点类似于职责链模式，通过链式调用这种类似同步的写法规范了异步代码的流程。

promise最具代表性的使用是在ajax请求方面的应用，我们可以大致看一下这段代码：

````javascript
var http = {
    get: function(url) {
        var promise = new Promise(function(resolve, reject) {
            $.ajax({
                url: url,
                method: 'get',
                success: function(data) {
                    resolve(data);
                },
                error: function(xhr, statusText) {
                    reject(statusText);
                }
            });
        });
        return promise;
    }
};
http.get('solve.php').then(function(data) {
    return data;
}, function(err) {
    return Promise.reject('Sorry, file not Found.');
}).then(function(data) {
    document.write(data);
}, function(err) {
    document.write(err);
});

作者：FangxinJiang
链接：http://www.jianshu.com/p/87183851756f
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
````

Promise是一个构造函数，此构造函数传入一个匿名函数作为参数，此函数传入2个函数作为此匿名函数的参数，这俩个函数是浏览器部署好的，异步操作则封装在此匿名函数当中，通过匿名函数的执行情况，分别执行resolve、reject函数，通知下一步继续进行操作。

下一步进行操作时，通过promise对象的then属性，执行需要进行的操作，then属性调用传入的2个函数，第一个函数在resolve被触发后执行，第二个函数在reject被触发后执行，这2个函数如果需要接受参数，可在执行resolve、reject时传入。

如果在调用了第一个then后还需要继续对数据进行处理，可以继续调用then，此时then中函数接收的参数需要上一个then中的函数返回。

- fetch中的promise

fetch请求会返回一个promise，用户可以在then中处理对应的情况,下方代码中的A和B的执行效果是一样的，但是B方案看起来更加的清晰。

````javascript
        var result = fetch('/demojson/demo.json');
        // A
        result.then(function(response){
            // 返回成功的结果
            console.dir(response);
        },function(response){
            // 返回失败的结果
            console.dir(response);
        });

        // B
        result.then(function(response){
            // 执行返回成功是的结果
            console.dir(response);
        }).catch(function(error){
            // 执行失败时的代码
            console.dir(error);
        });
````

- promise注意事项

1. promise 中then方法中执行的代码是异步的；
2. promise 中的状态分为 padding success fail，状态的转换过程为 padding->success 或 padding->fail,且状态的转换不可逆转；
3. promise 采用的是链式调用，但是每次调用返回的是上一次调用产生的新的promise。








