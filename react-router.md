## react-router 种的hashHistory browerHistory createMemoryHistory

> 最近着手开始改造一下公司的前端框架，查看了一下资料，对于react-router这一块儿，react-router由开始的默认hashHistory，改为推荐使用browerHistory ，在查找了一番资料后，我再将项目修改为browerHistory 之后，却发现了一些问题，在使用browerHistory 的时候，页面刷新之后，页面是无法展示的。

### hashHistory 

对于一个单页应用来说，如果访问的路由为```http://localhost:63342/demeter-wx/webapp/index-wx.html?_ijt=n33l11r3apf48oe8pqof2ckbpr#/addBlock```,那么我认为页面查找的过程应该是这样的，先根据```http://localhost:63342/demeter-wx/webapp/index-wx.html```找到对应的html文件，文件找到之后，再根据#后面的hash值来渲染对应的组件。

*优点：*  服务器不需要进行任何的配置，因为，首先访问html文件是很正常的访问操作，只有根据#后面的hash值进行不同的组件加载这是页面中js进行操作。
*缺点：* 不利于SEO优化，但是如果是做微信开发，感觉也没什么关系。

### browerHistory 

目前官方推荐使用的是browerHistory,其会调用浏览器的history API接口，其路由和正常的路由没有什么区别，在路由中不会出现key值。

*优点：*  每次访问都会去请求服务器，个人感觉browerHistory与history API接口直接存在一个映射关系。
*缺点：*  需要后台server支持。