## 在react中使用echarts

### step1

目前echarts已经有较多的react的实现，具体的可参见[echarts_github中的说明](https://github.com/ecomfe/echarts)

### 在使用中遇到的问题

在使用中遇到这样的问题：一般的统计的图表按照相应的react组件提供的文档即可进行开发，主要是在有地图图表的开发中遇到一些问题，主要是地图的使用中出现的问题。

一下提供一个组件的写法：

````javascript
import React from 'react';
import ReactEcharts from 'echarts-for-react';

require("echarts/map/js/province/guizhou.js");

const EchartsTest = React.createClass({
	propTypes: {},
	randomData: function () {
		return Math.round(Math.random() * 1000);
	},
	getOtion: function () {
		const option = {
			title: {
				text: 'iphone销量',
				subtext: '纯属虚构',
				left: 'center'
			},
			tooltip: {
				trigger: 'item',
				formatter: function (params, value, callback) {
					let $pna = params.name;
					let res = '';
					res = '<div class="echarts-container"><div class="echarts-item"></div><div class="echarts-item"></div><div class="echarts-item"></div><div class="echarts-item"></div><div class="echarts-item"></div><div class="echarts-item"></div></div>';
					return res
				}
			},
			legend: {
				orient: 'vertical',
				left: 'left',
				data: ['iphone3']
			},
			visualMap: {
				min: 0,
				max: 2500,
				left: 'left',
				top: 'bottom',
				text: ['高', '低'],           // 文本，默认为数值文本
				calculable: true
			},
			toolbox: {
				show: true,
				orient: 'vertical',
				left: 'right',
				top: 'center',
				feature: {
					dataView: {readOnly: false},
					restore: {},
					saveAsImage: {}
				}
			},
			series: [
				{
					name: 'iphone3',
					type: 'map',
					mapType: '贵州',
					roam: false,
					label: {
						normal: {
							show: true
						},
						emphasis: {
							show: true
						}
					},
					data: [
						{name: '六盘水', value: this.randomData()},
						{name: '贵阳', value: this.randomData()},
						{name: '遵义', value: this.randomData()},
						{name: '安顺', value: this.randomData()},
						{name: '铜仁', value: this.randomData()},
						{name: '毕节', value: this.randomData()},
						{name: '黔西南', value: this.randomData()},
						{name: '黔东南', value: this.randomData()},
						{name: '黔南', value: this.randomData()}
					]
				}
			]
		};
		return option;
	},
	render: function () {
		return (
			<div className='examples'>
				<div className='parent'>
					<label> render a china map. <strong>MAP charts</strong>: </label>
					<ReactEcharts
						option={this.getOtion()}
						style={{height: '500px', width: '100%'}}
						className='react_for_echarts'/>
				</div>
			</div>
		);
	}
});

export default EchartsTest;
````

需要安装echarts、echarts-for-react npm包；

其中需要注意的是：关于地图各个区块移入时才会显示，可以使用模板来显示数据。