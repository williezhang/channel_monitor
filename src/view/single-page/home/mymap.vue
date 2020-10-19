<template>
    <div ref="dom"></div>
</template>

<script>
import echarts from "echarts";
//   import '../../node_modules/echarts/map/js/world.js'
import 'echarts/map/js/china.js' // 引入中国地图数据
import { on, off } from '@/libs/tools'

export default {
data(){
    return {
        dom:null
        }
    },
methods:{
    resize () {
        this.dom.resize()
        }        
    },
mounted () {

    function randomValue() {
            return Math.round(Math.random()*1000);
        }

    this.loctest=window.localStorage.getItem('tableTitle')
    var all_data=eval(this.loctest)


    //x为字段，y为选择的月份，返回当月某个字段的枚举值，用来画饼图    
    var pick=function(x,y){
      var example_num={}
      for (var i=0,len=all_data.length;i<len;i++){
        if (all_data[i]['月份']==y) {
          if(example_num.hasOwnProperty(all_data[i][x])){example_num[all_data[i][x]]=example_num[all_data[i][x]]+1}
          else{example_num[all_data[i][x]]=1}
        }
      }
      var example_num_show=[]
      for(var key in example_num){ 
        //console.log(key)
         if(key!="undefined")            
            {
              example_num_show.push({value:example_num[key],name:key})
            }
          } 
      return  example_num_show  
    } 

    var month_detail=[]
    for (var i=0,len=all_data.length;i<len;i++){
      if (month_detail.indexOf(all_data[i]['月份']) === -1) {
                    month_detail.push(all_data[i]['月份']);
                }
    }
    var current_month=month_detail[month_detail.length-1]

    var province_count=pick('地域',current_month)
    
    var data_max=0
    var data_min=province_count[0]['value']
    for (var i=0,len=province_count.length;i<len;i++){
      if (province_count[i]['value']>data_max) {
                    data_max=province_count[i]['value']
                }
      if (province_count[i]['value']<data_min) {
                    data_min=province_count[i]['value']
                }               
    }    

    const option={
        backgroundColor: "rgba(0,0,0,0)",
        tooltip: {}, // 鼠标移到图里面的浮动提示框
        toolbox: {
              show: true,
              orient: 'vertical',
              left: 'right',
              top: 'center',
              feature: {
                  dataView: {readOnly: false},
                  restore: {},
              }
          },
        visualMap: {
            min: data_min,
            max: data_max,
            text: ['High', 'Low'],
            realtime: false,
            calculable: true,
            inRange: {
                color: ['lightskyblue', 'yellow', 'orangered']
            }
        },
          geo: { // 这个是重点配置区
            map: 'china', // 表示中国地图
            roam: true,
            label: {
              normal: {
                show: true, // 是否显示对应地名
                textStyle: {
                  color: 'rgba(0,0,0,0.4)'
                }
              }
            },
            itemStyle: {
              normal: {
                borderColor: 'rgba(0, 0, 0, 0.2)'
              },
              emphasis: {
                areaColor: null,
                shadowOffsetX: 0,
                shadowOffsetY: 0,
                shadowBlur: 20,
                borderWidth: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          },
          title:{text: '全国各省市进件分布情况',x: 'center', textStyle: {
            "color": "#516b91"
          }},
          series: [{
              type: 'scatter',
              coordinateSystem: 'geo' // 对应上方配置
            },
            {
              name: '进件量', // 浮动框的标题
              type: 'map',
              geoIndex: 0,
              data: province_count
            }
          ]    
    }
    this.$nextTick(() => {
        this.dom = echarts.init(this.$refs.dom)
        this.dom.setOption(option)
        on(window, 'resize', this.resize)
    })
    },
    beforeDestroy () {
        off(window, 'resize', this.resize)
    }
}
</script>