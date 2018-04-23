## vue计算属性

### vue.computed

vue的计算属性，在使用react的时候，因为使用了JSX，这在一定程度上模糊了html代码与js代码的边界，html页面也是有js生成的，你可以在render函数的JSX代码中写入各种你想写的js代码，感觉是不是很爽，在vue中每个组件一定程度上市区分了html、javascript、css三者，所以在vue的模板中写js的代码可能不会那么的方便。

但是以上的限制，从一定程度上来说，使vue的组件代码可能会更可读一点，当然前提是要对vue的api比较的属性，因为vue的作者为我们做了很多，包括vue.computed,该属性的作用是，当组件接受到的值，或是要显示的值与目前现有的数据还存在一定的差距（需要通过一定的计算才能使用），最常见的如时间格式化，从后台之间获取的数据可能是一个时间戳，在显示的时候可不能显示时间戳，我们可以通过computed属性，将原始数据计算为需要的数据，并绑定到模板当中。

### vue.watch

其实我在一开始的时候，对于watch属性用的不是很多，因为在vue内部数据与视图是双向绑定的，数据的改变会之间驱动视图的修改，那么监测数据的变化在那些场景下是有用的呢？

最近在一次将echarts组件用vue封装时感受到了他的用处，在echarts当中数据与视图并不是双向绑定的，为了实现数据的双向绑定，我使用watch,一下是封装的代码：

````javascript
<template>
    <div>
        <div class="echarts" :id="chartId"></div>
    </div>
</template>

<script>
    import echarts from 'echarts';
    export default {
        name: "common-chart",
        props: {
            options: {
                type: Object,
                required: true,
                default: function () {
                    return {

                    }
                }
            },
            chartId: {
                type: String,
                required: true,
                default: function () {
                    return 'chart'
                }
            }
        },
        data () {
            return {
                mychart: {}
            }
        },
        watch: {
            // 'options' :{
            //     handler:(val,oldVal)=>{
            //         console.log('数据发生了更新');
            //         console.dir(val);
            //         console.dir(oldVal);
            //         console.dir(this);
            //     },
            //     deep:true
            // }
            'options': function () {
                console.log('数据发生了变化');
                console.dir(this);
                this.updateCharts();
            }
        },
        methods: {
            initCharts () {
                let me = this;
                var myChart = echarts.init(document.getElementById(me._props.chartId));
                this.mychart = myChart;
                this.updateCharts();
            },
            updateCharts () {
                let me = this;
                me.mychart.setOption(me._props.options);
            }
        },
        mounted () {
            let me = this;
            me.initCharts();
            window.addEventListener('resize', function () {
                me.mychart.resize();
            });
        },
    }
</script>
````

可以看到，代码中我通过检测通过props传如的options配置数据的变化，来触发echarts配置的更新操作，在其中还发现了，通过声明式定义的触发函数并不能访问vue实例。

