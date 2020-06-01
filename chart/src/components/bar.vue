<template>
    <div id='chart'></div>
</template>
<script>
import echarts from 'echarts'
export default {
    methods:{
        init(){
            this.xdata = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
            this.ydata = [120, 200, 150, 80, 70, 110, 130];
            let symbolSize=40;
            let myself = this;
            let option = {
                xAxis: {
                    type: 'category',
                    data: this.xdata
                },
                yAxis: {
                    type: 'value',
                },
                series: [{
                    data: this.ydata,
                    type: 'bar',
                    name:'k'
                }]
            };
            let container = document.getElementById('chart');
            container.style.width = '800px';
            container.style.height = '800px';
            let mychart = echarts.init(container);
            mychart.setOption(option)
            let graphicValue = echarts.util.map(myself.ydata, function (dataItem, dataIndex) {
                return {
                    // 'circle' 表示这个 graphic element 的类型是圆点。
                    type: 'circle',

                    shape: {
                        // 圆点的半径。
                        r: symbolSize / 2
                    },
                    // 用 transform 的方式对圆点进行定位。position: [x, y] 表示将圆点平移到 [x, y] 位置。
                    // 这里使用了 convertToPixel 这个 API 来得到每个圆点的位置，下面介绍。
                    position: mychart.convertToPixel({'xAxisIndex':0,'yAxisIndex':0}, [myself.xdata[dataIndex],dataItem]),

                    // 这个属性让圆点不可见（但是不影响他响应鼠标事件）。
                    invisible: false,
                    // 这个属性让圆点可以被拖拽。
                    draggable: true,
                    // 把 z 值设得比较大，表示这个圆点在最上方，能覆盖住已有的折线图的圆点。
                    z: 100,
                    // 此圆点的拖拽的响应事件，在拖拽过程中会不断被触发。下面介绍详情。
                    // 这里使用了 echarts.util.curry 这个帮助方法，意思是生成一个与 onPointDragging
                    // 功能一样的新的函数，只不过第一个参数永远为此时传入的 dataIndex 的值。
                   ondrag: echarts.util.curry(myself.onPointDragging, dataIndex),
                   
                };
            })
            option.graphic=graphicValue
            this.option = option;
            this.mychart = mychart
            mychart.setOption(option);
        },
        props:{
            xdata:{
                type:Array,
                default(){
                    return []
                }
            },
            ydata:{
                type:Array,
                default(){
                    return []
                }
            },
        },
        onPointDragging(dataIndex,e){
            let myself = this
            let ydata = this.mychart.convertFromPixel({'yAxisIndex':0}, e.offsetY);
            this.$emit('changeYdata',dataIndex,ydata)
            //this.ydata[dataIndex] =this.mychart.convertFromPixel({'yAxisIndex':0}, e.offsetY);
            //this.option.graphic[dataIndex].position=this.mychart.convertToPixel({'xAxisIndex':0,'yAxisIndex':0}, [this.xdata[dataIndex],ydata])
            this.option.series[0].data[dataIndex] = ydata;
            let graphicValue = echarts.util.map(myself.ydata, function (dataItem, dataIndex) {
                return {
                    // 'circle' 表示这个 graphic element 的类型是圆点。
                    type: 'circle',

                    shape: {
                        // 圆点的半径。
                        r: 40 / 2
                    },
                    // 用 transform 的方式对圆点进行定位。position: [x, y] 表示将圆点平移到 [x, y] 位置。
                    // 这里使用了 convertToPixel 这个 API 来得到每个圆点的位置，下面介绍。
                    position: myself.mychart.convertToPixel({'xAxisIndex':0,'yAxisIndex':0}, [myself.xdata[dataIndex],dataItem]),

                    // 这个属性让圆点不可见（但是不影响他响应鼠标事件）。
                    invisible: false,
                    // 这个属性让圆点可以被拖拽。
                    draggable: true,
                    // 把 z 值设得比较大，表示这个圆点在最上方，能覆盖住已有的折线图的圆点。
                    z: 100,
                    // 此圆点的拖拽的响应事件，在拖拽过程中会不断被触发。下面介绍详情。
                    // 这里使用了 echarts.util.curry 这个帮助方法，意思是生成一个与 onPointDragging
                    // 功能一样的新的函数，只不过第一个参数永远为此时传入的 dataIndex 的值。
                   ondrag: echarts.util.curry(myself.onPointDragging, dataIndex),
                   
                };
            })
            this.option.graphic=graphicValue
            this.mychart.setOption(this.option)
        },
    },
    mounted(){
        this.init()
    },
    data(){
       return {
            option:{},
            mychart:{},
        }
    }
   
}
</script>
<style scoped>

</style>