# Set and Map

> 学习一个知识点之前习惯的想找找这个知识点解决了什么问题，但是到目前位置Set 和 Map 暂时还没有找到适用的场景，但是我还是决定将其学习以下，因为在java和python中常常会遇到Set和Map,Map可以看作是一种类似对象的数据类型，Set可以看作是一种类似于数组的数据类型。

* Set

使用了set之后我们是否可以舍弃数组呢？目前来看可以的,基本上可以将set看作是数组的一个扩展，他与数组的区别在哪里呢，在数组中你是可以添加重复的元素的，但是在Set当中是不可以的，在Set当中是不可能同时存在2个相同的元素的，但是是可以存在2个值相等的引用类型数据，只要他们的内存地址是不相同的。

Set又这样的几个方法属性：

* add\(value\):添加某个值，返回Set结构本身
* delete\(value\):删除某个值，返回一个布尔值，用于表示是否删除成功，需要注意的是如果要伤处对象元素，需要保证删除时使用的对象的内存地址与Set中对象元素的内存地址是相同的
* has\(value\):
