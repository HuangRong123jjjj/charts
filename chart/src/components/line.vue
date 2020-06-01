<template>
    <!-- 折线图 -->
    <div :id="id" :class="className" :config="config" 
    :data="data" :style="{ height:height, width:width }" 
    :seriesNames="seriesNames"/>
</template>
<script>
  import echarts from 'echarts'
  export default {
     props: {
      id: {
        type: String,
        default: 'chart'
      },
      className: {
        type: String,
        default: 'chart'
      },
      width: {
        type: String,
        //default: '268px'
        default: '288px'
      },
      height: {
        type: String,
        default: '300px'
      },
      config: {
        type: Object,
        default: null
      },
      data:{
        type: Array,
        default: ()=>[]
      },
      xData:{
        type:Array,
        default:()=>{return ['5/1', '5/2', '5/3', '5/4', '5/5', '5/6', '5/7', '5/8', '5/9', '5/10', '5/11', '5/12','5/13', '5/14', '5/15', '5/16']}
      },
      seriesNames:{
        type:Array,
        default:()=>['每日总单量', '日平均总单量']
      },
      axisLabel: {
         type:Object,
         default:()=>{
           return {
              rotate: 45,
           }
         }
      },
    },
    data() {
      return {
        chart:null
      }
    },
    updated(){
      let res = document.getElementById(this.id);
      res.style.width = window.innerWidth/2-360+'px';
      this.chart = echarts.init(res)
      this.initChart(this.chart,this.config,this.data)
      this.chart.resize()
    },
     beforeDestroy() {
      if (!this.chart) {
        return
      }
      this.chart.dispose()
      this.chart = null
    },
    methods: {

      initChart(myChart,config,chartData){

       /*var xData = function() {
           var data = [];
           for (var i = 1; i < 31; i++) {
               data.push(i + "日");
           }
           return data;
       }();*/
       myChart.clear();
       let series=[];
       let itemColors=['#5fbdff','#F6C541','bbb','#af7df3','aaa','#bf7d8e','f1f2f3','#cfffe3']
       for(let i =0;i<this.seriesNames.length;i++){
         series.push(
           {
               name: this.seriesNames[i],
               type: "line",
               //折线图点的大小
               symbolSize: 5,
               symbol: 'circle',
               itemStyle: {
                   color: itemColors[i],
               },
               data:chartData[i],
           }
         )
       }
       const option = {
           backgroundColor: "#ffffff",

           tooltip: {
                trigger: "axis",
                axisPointer: {
                    //柱线显示
                    type: "shadow"
                    //竖线显示
                    //type:"line"
                },
                textStyle: {
                    color: "#fff"
                }
           },
           grid: {
               borderWidth: 0,
               left: 70,
               right:10,
               top: 10,
               bottom: 85,
               textStyle: {
                   color: "#fff"
               }
           },
           legend: {
            left: '55px',
            bottom: '5px',
            textStyle: {
              color: '#90979c',
            },
            data: this.seriesNames
           },
           calculable: true,
           xAxis: [{
               type: "category",
               axisLine: {
                   lineStyle: {
                       color: '#000000',
                   }
               },
               splitLine: {
                   show: false
               },
               //是否显示坐标轴刻度。
               axisTick: {
                   show: false
               },
               data: this.xData,
               axisLabel:this.axisLabel,
           }],

           yAxis: [{
               type: "value",
               splitLine: {
                   show: false
               },
               axisLine: {
                   lineStyle: {
                        width: 3,
                        color: {
                            type: 'linear',
                            x: 0,
                            y: 0,
                            x2: 0,
                            y2: 1,
                            colorStops: [{
                                offset: 0,
                                color: 'red'
                            }, {
                                offset: 1,
                                color: 'blue'
                            }],
                            globalCoord: false // 缺省为 false
                        }
                   }
               },

           }],
           dataZoom: [{
               show: true,
               height: 16,
               xAxisIndex: [0],
               bottom: 30,
                //滚动条起始位置
               "start": 0,
               //滚动条中止占比
               "end": (10/this.xData.length) * 100,
               handleIcon: 'path://M306.1,413c0,2.2-1.8,4-4,4h-59.8c-2.2,0-4-1.8-4-4V200.8c0-2.2,1.8-4,4-4h59.8c2.2,0,4,1.8,4,4V413z',
               handleSize: '100%',
               handleStyle: {
                   color: "#71ACFC",
               },
               textStyle:{
                   color:"rgba(204,187,225,0.9)",
               },
               fillerColor:"rgba(82,155,255,0.8)",
               borderColor: "rgba(204,187,225,0.5)",

             }, {
               type: "inside",
               show: true,
               height: 15,
               start: 1,
               end: 35
           }],
           series:series
       }
        myChart.setOption(option);
      }
    },
  }
</script>
