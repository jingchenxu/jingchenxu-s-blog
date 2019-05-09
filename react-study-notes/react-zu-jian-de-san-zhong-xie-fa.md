# react 组件的三种写法

## 第一种写法

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
})
```

这种写法是类似与写配置文件的一种写法，你会发现这种写法和vue的写法特别的类似，当时使用react的版本是0.14的版本，这个文章的补完确实跨度比较大，到现在才开始补充。

## 使用ES6创建组件类

这种方式创建组件迎合了部分Java用户的喜好，我对于class的出现还是接受的，但是总的来说和原来的创建组件的方式区别还是比较大的，感觉react有点步子大了：

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import './BottomNavigator.css'

class BottomNavigator extends React.Component {
   constructor(props){
       super(props)
       this.state = {
           naviList: [{
               name: '首页',
               path: 'indexpage'
           },{
               name: '用户中心',
               path: 'usercenter'
           },{
               name: '购物车',
               path: 'shopingcart'
           }]
       }
   } 
   render () {
       return (
           <div className="bottom-navigator">
           <div className="navi-container">
               {
                   this.state.naviList.map((item, index) => <Link key={index} to={item.path}><div className="navi-item">{item.name}</div></Link>)
               }
           </div>
           </div>
       )
   }
   handleLinkTo (route) {
     console.dir(route)
     location.href = route.path
   }
}

export default BottomNavigator
```

## 创建无状态组件

```javascript
import React from 'react'
import { BrowserRouter as Router, Route, Link } from 'react-router-dom'
import './app.css'
import HelloWorld from './components/HelloWorld'
import PageA from './components/PageA'
import BottomNavigator from './components/BottomNavigator'
import IndexPage from './pages/IndexPage'
import UserCenter from './pages/UserCenter'
import ShopingCart from './pages/ShopingCart'

function App() {
  return (
    <Router>
      <Route exact path="/" component={HelloWorld} />
      <Route path="/pagea/" component={PageA} />
      <Route path="/indexpage" component={IndexPage} />
      <Route path="/usercenter" component={UserCenter} />
      <Route path="/shopingcart" component={ShopingCart} />
      <BottomNavigator/>
    </Router>
  )
}

export default App
```

