# vue实现动态表单

### 需求在哪里

不得不说用vue开发后台管理系统确实是个比较蛋疼的事情，后台管理系统最多的页面是列表详情页，在详情页上可能有更多的子表，是的有一堆的子表，我这里针对的就是列表详情页的子表这样的需求，如果我们正常的操作，自然是在详情页写上子表的列表，通过modal弹窗来实现列表数据的新增与删除。

### 如何将这部分的业务进行抽象

关于这部分的业务网上是有很多的讨论的，比较多的解决方案是，是通过后台接口传递过来的数据动态的渲染出表单页面，当前的系统设计是列表页的表头数据是由后台接口提供的，所以可以在表头数据中设置每个字段的信息。项目是由vue-cli生成的，使用iview作为默认UI库，所以接下来我实现的动态表单会基于iview的组件。



