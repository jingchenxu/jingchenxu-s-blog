## react组件的三种写法

### 第一种写法

> 该写法在react16.0.0中已不支持，如果想继续使用这种写法，可以引入单独的reac-creact-class npm包。

```javascript
module.exports = React.createClass({
displayName: 'Assistant',
render: function () {
var AssistantList = ItemList(AssistantItem, '', {
scrollPaging: true
});
return(
<div style={{paddingTop: '70px'}}>
<div style={{width: '100%',height: '100px',margin: '50px 200x 0 20px'}}>
暂无内容
</div>
</div>
)
}
conponentDidMount: function () {
console.log('组件加载完成');
}
});
```

> 这种写法刚开始接触react的时候的写法，当时采用的react的版本是0.14版本的，