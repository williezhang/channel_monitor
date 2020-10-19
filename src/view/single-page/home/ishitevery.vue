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

    this.loctest=window.localStorage.getItem('tableTitle')
    var all_data=eval(this.loctest)

    //保留四位小数
    var to4float=function(number){
      number = String(number*100).replace(/^(.*\..{2}).*$/,"$1");
      return number
    }

    //分箱函数
    var split_bin=function(x){
      if(x<2){return '0~1'}
      else if(x<4){return '2~3'}
      else if(x<6){return '4~5'}
      else if(x<8){return '6~7'}
      else if(x>=8){return '8+'}
      else{}
    }    

    //颜色
    var colorList = [
                    "#FE2E2E", "#2d8cf0","#04B45F","#2d8cf0","#19be6b","#9A66E4","#ed3f14"
                    ]
    //确定月份
    var month_detail=[]
    for (var i=0,len=all_data.length;i<len;i++){
      //console.log(all_data[i]['在网时长(使用时间分数)'],typeof(all_data[i]['在网时长(使用时间分数)']))
      if (month_detail.indexOf(all_data[i]['月份']) === -1) {
                    month_detail.push(all_data[i]['月份']);
                }
    }
 //   console.log(month_detail)
    var current_month=month_detail[month_detail.length-1]

    var legend_list=['命中银行机构','命中消费金融机构','命中p2p或者小贷机构']
    var bin_sort=["0~1","2~3","4~5", "6~7", "8+"]
    //console.log(month_detail,current_month)
    var series_list=[] 
    for(var i=0,len=legend_list.length;i<len;i++){
        var ret= {'name': legend_list[i],'type': 'bar','color':colorList[i],'label':{'show':true,'position': 'top','formatter': '{c}'}}
        var data1={'0~1':0,'2~3':0,'4~5':0,'6~7':0,'8+':0}
        for (var j=0,len1=all_data.length;j<len1;j++){
            if(all_data[j]['月份']==current_month){
                var cur_hit=split_bin(all_data[j][legend_list[i]+'数'])
                //console.log(cur_hit,all_data[j][legend_list[i]])
                data1[cur_hit]=data1[cur_hit]+1
            }
        }
        ret['data']=[data1['0~1'],data1['2~3'],data1['4~5'],data1['6~7'],data1['8+']]
        series_list.push(ret)  
    }   

 //   console.log(series_list)


    //  for (var i=0,len=all_data.length;i<len;i++){
    //      var sub_month=all_data[i]['月份']
    //      if(sub_month in xaxis_data){
    //         var bin=split_bin(all_data[i]['命中机构数目'])
    //         if(xaxis_data[sub_month].hasOwnProperty(bin)){xaxis_data[sub_month][bin]=xaxis_data[sub_month][bin]+1}
    //         else{xaxis_data[sub_month][bin]=1}
    //      }
    //    }
    

    // //var month_sort=(Object.keys(xaxis_data)).sort()
    // var month_sort=month_list.sort()
    // var bin_sort=["0~4","5~9","10~14", "15~19", "20+"]


    // console.log(xaxis_data,month_sort,bin_sort)
    // var series_list=[]

    // for(var i=0,len=month_sort.length;i<len;i++){
    //     var ret= {'name': month_sort[i],'type': 'bar','color':colorList[i]}
    //     var data1=[]
    //     for (var j=0,len1=bin_sort.length;j<len1;j++){
    //       data1.push(xaxis_data[month_sort[i]][bin_sort[j]]) 
    //     } 
    //     ret['data']=data1
    //     series_list.push(ret)  
    // }

    const option = {
    backgroundColor: "rgba(0,0,0,0)",
    title:{text: '本月命中各机构数分箱对比',x: 'center', textStyle: {
            "color": "#516b91"
        }},
    legend: {
              orient: 'horizontal',
              right: 'right',
              data:legend_list          
            },
    tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} '
    },
      xAxis: [
        {
          data: bin_sort,
          splitLine:{show: false},
          name:'命中机构分箱',
          nameTextStyle:{
            fontSize:8,
            },
        }
      ],
      yAxis: [
        {
          type: 'value',
          splitLine:{show: false},
          name:'人数',
          nameTextStyle:{
            fontSize:8.5,
            },
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

