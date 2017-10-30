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







