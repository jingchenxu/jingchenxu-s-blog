# 前端与后端交互的几种方式

> 本文中的所有实践前端请求采用的是axios,后端使用的是springMVC,集中讨论的是get、post俩中请求方式，对于各种场景下的前后端交互在这里进行一个总结，本文以各种实际使用场景为例。

* 数据查询

我们常常会遇到数据查询的情况，这时候后端接收的参数多为一些查询参数，参数的数量不是很大，这种情况下我们会将参数添加在接口请求的url当中，下面我们以axios请求为例：

```javascript
axios.get('/api/v1/getdemolist, {
    params: {
        pageNo: 1,
        pageSize: 10
    }
}).then(function(){
         // TODO
    })
```

后端springMVC接受参数的方式为\(共计有3中，可选择合适的\):

```java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(int pageNo, int pageSize) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
```

这种方式可以将请求参数自动的与形参进行对应，无需多余的操作。

```java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(Page page) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
```

这种方式可以自动的将参数对应到bean的对应属性，这样的好处是，如果参数已一个bean当中的参数，则客户自动的创建bean对应的实例，并将请求参数填充到对应的属性当中。

```java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(@RequestParam("pageNo") int pageNo, @RequestParam("pageSize") int pageSiz) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
```

这种方式是通过@RequestParam完成请求参数与形参的绑定，该注解还具有的额外功能就是，指定那些参数是非必须的，那些参数是必须的。

对于get请求，前端请求参数最好还是通过resetful的方式，当然也可以使用占位符的方式，但是就是没有这么灵活罢了。

采用占位的方式则是☞将参数作为url的一部分，进行请求，从而获取数据，具体的前端ajax请求示例如下：

```javascript
axios.get('/api/v1/getdemolist/1/10')
    .then(function(){
         // TODO
    })
```

后端接收数据的方式为：

```java
@RequestMapping("/getdemolist/{pageNo}/{pageSiz}")
@ResponseBody
public ReturnValue getDemoList(@PathVariable("pageNo") int pageNo, @PathVariable("pageSize") int pageSiz) {
// 获取监测点列表表头
ReturnValue rtv = new ReturnValue();
rtv.setSuccess(true);
// TODO
return rtv;
}
```

以上使我们在数据查询时常用的一些方式。

* 数据保存及更新

在数据保存及更新时我们通常会使用post的请求方式，应为使用get的请求方式，对于url的长度是有限制的，同时使用post请求可以尽量避免中文乱码的问题。

post请求前端示例如下：

```javascript
axios.post('/api/v1/getdemolist, {
    pageNo: 1,
    pageSize: 10
}).then(function(){
         // TODO
    })
```

后端可以通过以下2中方式进行参数接收:

```java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(int pageNo, int pageSize) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
```

```java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(@RequestBody Page page) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
```

以上是正常的post提交，但是form表单也是可以进行post提交的，但是提交的数据格式可能不是很一样，而且我们要考虑到form表单提交时，可能会提交多个bean对应的数据，所以我们需要添加一些参数前缀：

我们可以直接进行form表单的提交，也可以使用工具函数将json对象转化为form表单对象进行提交，form表单的提交大家应该都知道，这里我们通过一个转化函数，将json对象转化为form表单对象：

转化函数如下（该函数会添加参数前缀，用于接下来的后端的参数接收）：

```javascript
util.addPreParams = function (params, pre) {
    let result = new URLSearchParams();
    for (let key in params) {
        result.append(`${pre}.${key}`, params[key]);
    }
    return result;
};
```

开始进行数据请求：

```javascript
let data = {
    pageNo: 1,
    pageSize: 10
};
axios.post('/api/v1/getdemolist, util.addPreParams(data, 'page')).then(function(){
         // TODO
    })
```

后端进行数据接收：

```java
@Controller
@RequestMapping("v1")
public class SiteController {

    @RequestMapping("getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(@ModelAttribute("page") Page page) {
        ReturnValue rtv = new ReturnValue();
        // TODO
        return rtv;

    }

    @InitBinder("page")
    public void InitBinderBasSite(WebDataBinder binder) {
        binder.setFieldDefaultPrefix("page.");
    }
}
```

通过这样的方式，可以将一次性提交的数据，绑定到多个对应的bean上，减少在数据转换上要写的代码。

-

