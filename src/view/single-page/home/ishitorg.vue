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
        dom: null
        }
    },
    methods: {
        resize () {
            this.dom.resize()
        }
    },
    mounted () {

    this.loctest=window.localStorage.getItem('tableTitle')
    var all_data=eval(this.loctest)
    //console.log(all_data)

    //保留四位小数
    var to4float=function(number){
      number = String(number*100).replace(/^(.*\..{2}).*$/,"$1");
      return number
    }

    //分箱函数
    var split_bin=function(x){
      if(x<5){return '0~4'}
      else if(x<10){return '5~9'}
      else if(x<15){return '10~14'}
      else if(x<20){return '15~19'}     
      else if (x>=20){return '20+'}
      else{}
    }    

    //颜色
    var colorList = [
                    "#FE2E2E", "#2d8cf0","#04B45F","#2d8cf0","#19be6b","#9A66E4","#ed3f14"
                    ]
    //确定月份
    var month_detail=[]
    for (var i=0,len=all_data.length;i<len;i++){
      if (month_detail.indexOf(all_data[i]['月份']) === -1) {
                    month_detail.push(all_data[i]['月份']);
                }
    }


 //   console.log(month_detail)
    var current_month=month_detail[month_detail.length-1],last_month=month_detail[month_detail.length-2],last_last_month=month_detail[month_detail.length-3]
    var month_list=[current_month,last_month,last_last_month]


    //var xaxis_data={'currnt_month':{'真实身份':0,'虚假身份':0,'未知':0},'last_month':{'真实身份':0,'虚假身份':0,'未知':0},'history':{'真实身份':0,'虚假身份':0,'未知':0}}
    var xaxis_data={}
    for(var i=0,len=month_list.length;i<len;i++){
        xaxis_data[month_list[i]]={}
    }
     for (var i=0,len=all_data.length;i<len;i++){
         var sub_month=all_data[i]['月份']
         if(sub_month in xaxis_data){
            var bin=split_bin(all_data[i]['命中机构数目'])
            if(xaxis_data[sub_month].hasOwnProperty(bin)){xaxis_data[sub_month][bin]=xaxis_data[sub_month][bin]+1}
            else{xaxis_data[sub_month][bin]=1}
         }
       }
 
    //var month_sort=(Object.keys(xaxis_data)).sort()
    var month_sort=month_list.sort()
    var bin_sort=["0~4","5~9","10~14", "15~19", "20+"]

    //各月有命中机构的人数
    var month_detail_count={}
    for(var key1 in xaxis_data){
        var sum_=0
        for(var j=0,len=bin_sort.length;j<len;j++){
            sum_=sum_+xaxis_data[key1][bin_sort[j]]
        }
        month_detail_count[key1]=sum_
    }




    //console.log(xaxis_data,month_sort,bin_sort)
    var series_list=[]
    for(var i=0,len=month_sort.length;i<len;i++){
        var ret= {'name': month_sort[i],'type': 'bar','color':colorList[i],'label':{'show':true,'position': 'top','formatter': '{c}%'}}
        var data1=[]
        for (var j=0,len1=bin_sort.length;j<len1;j++){
          data1.push(to4float(xaxis_data[month_sort[i]][bin_sort[j]]/month_detail_count[month_sort[i]]))
        } 
        ret['data']=data1
        series_list.push(ret)  
    }

    const option = {
    backgroundColor: "rgba(0,0,0,0)",
    title:{text: '各月命中机构数分箱对比',x: 'center', textStyle: {
            "color": "#516b91"
        }},
    legend: {
              orient: 'horizontal',
              right: 'right',
              data:month_sort          
            },
    tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c}%'
    },
      xAxis: [
        {
          data: bin_sort,
          splitLine:{show: false},
          name:'命中机构分箱',
        }
      ],
      yAxis: [
        {
          type: 'value',
          splitLine:{show: false},
          name:'人数',
        }
      ],

      series: series_list
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

