## vue计算属性

### vue.computed

vue的计算属性，在使用react的时候，因为使用了JSX，这在一定程度上模糊了html代码与js代码的边界，html页面也是有js生成的，你可以在render函数的JSX代码中写入各种你想写的js代码，感觉是不是很爽，在vue中每个组件一定程度上市区分了html、javascript、css三者，所以在vue的模板中写js的代码可能不会那么的方便。

但是以上的限制，从一定程度上来说，使vue的组件代码可能会更可读一点，当然前提是要对vue的api比较的属性，因为vue的作者为我们做了很多，包括vue.computed,该属性的作用是，当组件接受到的值，或是要显示的值与目前现有的数据