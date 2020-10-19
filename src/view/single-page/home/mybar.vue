<template>
    <div ref="dom">
    </div>
</template>

<script>
import echarts from 'echarts'
import { on, off } from '@/libs/tools'

export default {
    data() {
      return {
        dom1: null
        }
    },
    methods: {
        resize () {
            this.dom.resize()
        }
    },
    mounted () {

    this.loctest1=window.localStorage.getItem('tableTitle')
    var all_data1=eval(this.loctest1)
    //console.log(all_data1)
    var xaxis_data={'currnt_month':{'真实身份':0,'虚假身份':0,'未知':0},'last_month':{'真实身份':0,'虚假身份':0,'未知':0},'history':{'真实身份':0,'虚假身份':0,'未知':0}}
     for (var i=0,len=all_data1.length;i<len;i++){
           if(all_data1[i]['月份']=='9月' & all_data1[i]['是否真实身份'] in xaxis_data['currnt_month']){  
              xaxis_data['currnt_month'][all_data1[i]['是否真实身份']]=xaxis_data['currnt_month'][all_data1[i]['是否真实身份']]+1      
            }
            else if(all_data1[i]['月份']=='8月' & all_data1[i]['是否真实身份'] in xaxis_data['currnt_month']){  
              xaxis_data['last_month'][all_data1[i]['是否真实身份']]=xaxis_data['last_month'][all_data1[i]['是否真实身份']]+1      
            } 
            else if (all_data1[i]['月份']=='7月' & all_data1[i]['是否真实身份'] in xaxis_data['currnt_month']){  
              xaxis_data['history'][all_data1[i]['是否真实身份']]=xaxis_data['history'][all_data1[i]['是否真实身份']]+1      
            }
            else{}  
       }
      
    var series_list=[]
    for(var key1 in xaxis_data){
      var ret=[]
      for (var key2 in xaxis_data[key1]){
        ret.push(xaxis_data[key1][key2]) 
      }
    series_list.push(ret)  
    }

  //  console.log(series_list)
    const option = {
    backgroundColor: "rgba(0,0,0,0)",
    title:{text: '是否真实身份',x: 'center', textStyle: {
            "color": "#516b91"
        }},
    legend: {
              orient: 'horizontal',
              right: 'right',
              data:['9月', '8月', '7月']           
            },
    tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} '
    },
      xAxis: [
        {
          data: ['真实身份', '虚假身份', '未知'],
          splitLine:{show: false}
        }
      ],
      yAxis: [
        {
          type: 'value',
          splitLine:{show: false}
        }
      ],

      series: [
        {
          name: '本月',
          type: 'bar',
          data: series_list[0],
          color:"#2d8cf0"

        },
        {
          name: '上月',
          type: 'bar',
          data: series_list[1],
          color:"#19be6b",
        },
        {
          name: '历史平均',
          type: 'bar',
          data: series_list[2],
          color:"#ff9900"
        },
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

