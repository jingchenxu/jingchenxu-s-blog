## 前端与后端交互的几种方式

> 本文中的所有实践前端请求采用的是axios,后端使用的是springMVC,集中讨论的是get、post俩中请求方式，对于各种场景下的前后端交互在这里进行一个总结，本文以各种实际使用场景为例。

- 数据查询

我们常常会遇到数据查询的情况，这时候后端接收的参数多为一些查询参数，参数的数量不是很大，这种情况下我们会将参数添加在接口请求的url当中，下面我们以axios请求为例：

````javascript
axios.get('/api/v1/getdemolist?pageNo=1&pageSize=10')
    .then(function(){
         // TODO
    })
````

后端springMVC接受参数的方式为(共计有3中，可选择合适的):

````java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(int pageNo, int pageSize) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
````

这种方式可以将请求参数自动的与形参进行对应，无需多余的操作。

````java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(Page page) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
````

这种方式可以自动的将参数对应到bean的对应属性，这样的好处是，如果参数已一个bean当中的参数，则客户自动的创建bean对应的实例，并将请求参数填充到对应的属性当中。

````java
    @RequestMapping("/getdemolist")
    @ResponseBody
    public ReturnValue getDemoList(@RequestParam("pageNo") int pageNo, @RequestParam("pageSize") int pageSiz) {
        // 获取监测点列表表头
        ReturnValue rtv = new ReturnValue();
        rtv.setSuccess(true);
        // TODO
        return rtv;
    }
````
这种方式是通过@RequestParam完成请求参数与形参的绑定，该注解还具有的额外功能就是，指定那些参数是非必须的，那些参数是必须的。

对于get请求，前端请求参数最好还是通过resetful的方式，当然也可以使用占位符的方式，但是就是没有这么灵活罢了。




- 