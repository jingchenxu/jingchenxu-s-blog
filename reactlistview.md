## react listView组件实现

### 如何实现移动端的滑动分页加载？

如何实现这是个问题，将这个问题进行转化，如何判断页面滑动到底部，触发分页数据的请求是解决问题的关键，得益于react封装了在数据更新情况下对dom的操作，我们只需要改变页面数据，即可完成对页面的重新渲染。

### 分页数据的处理
```javascript
//通过ajax请求获取到最新一次请求的分页数据
var _dataList = response.data.list;
var prv_dataList = this.state.dataList;
console.log('当前的页数为：' + pageNumber);
this.setState({
    //每页的数据都是array数据，新的数据通过concat添加到原来的数据，通过改变state，自动完成页面的渲染
    dataList: prv_dataList.concat(_dataList)
})
```

### 如何判断滑动到页面底部

首先我们获取客户端高度，即可视高度，我们所能看到的屏幕高度,这个一般是滑动列表外部滑块的所在的container的高度；

```javascript
var clientHeight = document.documentElement.clientHeight;
```

接下来我们获取内部的数据列表的高度，这个高度为数据内容的高度；

```javascript
var scrollHeight = document.documentElement.scrollHeight;
```

最后我们获取滚动条距离顶部的距离；

```javascript
var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
```

判断页面滑动到底部的方法为：

```javascript
scrollTop + clientHeight == scrollHeight
```

### 完整组件代码

```javascript
/**
 * Created by jcxu on 2016/11/9.
 * discription:
 */
let OrderStatus = require('../OrderStatus');

let HistoryItem = React.createClass({

	displayName: 'HistoryItem',

	render: function () {
		return (
			<div className="history-container">
				<div className="status">
					<span className={this.props.orderstatus=='02'?'order-status':'order-status-noBackground'}>{OrderStatus[this.props.orderstatus]}</span><span className="right">订单号：{this.props.orderid}</span>
				</div>
				<div className="description" onClick={this.linkTo}>
					<div className="ins">
						<div>收货人： {this.props.addcontact} {this.props.contactsex=='01'?'先生':'女士'}</div>
						<div>手机号： {this.props.addmobile}</div>
						<div>订单时间： {formatDataNow(this.props.createdate)}</div>
						<div style={{height: 40}}>收货地址： {this.props.addprov+' '+
						this.props.addcity+' '+
						this.props.addcounty+' '+
						this.props.addroad+' '+
						this.props.adddetail}</div>
					</div>
					<div className="more">
						<a href={'#/historyDetail/'+this.props.orderid}>
							<img style={{width: 10,margin: '9px 0 0 3px'}} src="img/right-arrow.png" alt=""/>
						</a>
					</div>
				</div>
			</div>
		)
	},

	linkTo: function () {
		location.href='#/historyDetail/'+this.props.orderid
	}

});

let HistoryMore = {

    displayName: 'HistoryMore',

    getInitialState: function () {
        return ({
            dataList: [],
            pageNumber: 1,
            isFirst: true//判断是不是第一次到达底部
        })
    },

    render: function () {

        return (
            <div ref="itemList">
                <div style={{ width: '100%', height: 'auto',paddingTop: '54px' }} >
                    {
                        this.state.dataList.length!=0 ? this.state.dataList.map((item, i) => {
                            return (
                                <HistoryItem {...item} key={i}/>
                            )
                        }) : ''
                    }
                    {
                        this.state.isFirst ? '' : <div style={{ width: '100%', height: '50px',marginTop: '20px', lineHeight: '50px', textAlign: 'center', border: '1px solid #018ee0' }}>没有更多数据了！</div>
                    }
                </div>

            </div>
        );
    },

    componentDidMount: function () {
        window.addEventListener('scroll', this.handleScroll);
        this.getData(1)
    },
    componentWillUnmount: function () {
        window.removeEventListener('scroll', this.handleScroll);
    },
    handleScroll: function (e) {
        //console.log('浏览器滚动事件');
        //console.dir(e);
        //距离顶部的距离
        var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop;
        //console.log('scrollTop:  ' + scrollTop);
        //可滑动的高度
        var scrollHeight = document.documentElement.scrollHeight;
        //console.log('scrollHeight:  ' + scrollHeight);
        //客户端gaodu
        var clientHeight = document.documentElement.clientHeight;
        //console.log('clientHeight:  ' + clientHeight);
        if (scrollTop + clientHeight == scrollHeight) {
            console.log('++++++++++++++++++++++++')
            if (this.state.isFirst) {
                var pre_pageNumber = this.state.pageNumber;
                this.setState({
                    pageNumber: pre_pageNumber + 1
                })
                this.getData(pre_pageNumber + 1)
            }
            else {
                alert('没有更多数据了！');
            }
        }
    },
    getData: function (pageNumber) {
        //console.log('获取数据开始执行了！');
        //'../../../demo/nestle/history' + pageNumber + '.json'
        //'shop/SearchOrdBase/'+custInformation.custid+'/'+pageNumber
        $.get('shop/SearchOrdBaseLimit/'+custInformation.custid+'/'+pageNumber, function (date) {
            var response = $.parseJSON(date);
            if (response.success) {
                var list = response.data.list;
                if (list.length != 0) {
                    //console.log(response.message);
                    var _dataList = response.data.list;
                    var prv_dataList = this.state.dataList;
                    console.log('当前的页数为：'+pageNumber);
                    this.setState({
                        dataList: prv_dataList.concat(_dataList)
                    })
                }
                else {
                    this.setState({
                        isFirst: false
                    })
                }
            }
            else {
                this.setState({
                    isFirst: false
                })
            }
        }.bind(this))
    }

};


module.exports = React.createClass(HistoryMore);

```



