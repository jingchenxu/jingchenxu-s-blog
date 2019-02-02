# 如何体面的从vue向react迁移

> 这个文章想写很久了，但是一直比较懒，vue是一个会将人变得更懒的框架，你想要的他都有，vue是个很好的框架，但是我与react的余情未了，所以这又回来骚扰骚扰了，之前使用react有一年半的时间，我前端入门就是react，v16前的版本都用过，api变化很大，说实在的all in js我感觉是不错的。

## 创建react项目

vue，react都有官方维护的项目生成工具，creat-react-app, vue-cli,这2个工具都是很不错的，create-react-app也集成了react的思想，只提供了最核心的功能，而像是react-router,redux之类的需要自己集成，集成过程中多少会有些问题，需要自己查阅相关的文档解决。

## 创建一个react组件

除了jsx外，react的其他方面都来自js自身，没有创造出什么新的概念，一个react组件就是一个js文件，然后你就开始写js,这样的好处有这样几点：
1. 了解下jsx,再这个文件里你写的都是js代码，样式也可以用js写；
2. 单个文件中可以写多个react组件，因为一个组件就是一个类；
3. 你可以看到类的构造函数；
但是你可能会遇到一些困惑的地方：
1. props的数据类型校验需要一个第三方包，是的react只管最核心的部分，其他的你需要向再淘宝找东西样淘配件，这让你知道，react中没有注定，没有都安排好了这样的说法；
2. 组件方法没有绑定this,我们要区分下，render函数以及生命周期函数都绑定了this,因为这是框架自带的，但是我们写的方法函数是没有绑定this的，所以this需要手动绑定(可以引入装饰器实现自动绑定this)：

````javascript
class Menu extends Component {
  constructor(props) {
    super(props)
    this.handleLink = this.handleLink.bind(this)
    this.state = {}
  }
  render() {
    return (
      <div onClick={this.handleLink} className="Menu">{this.props.menu.name}</div>
    )
  }
  handleLink() {
    console.log('menu被点击')
    console.dir(this)
  }
}
````
