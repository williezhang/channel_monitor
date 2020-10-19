<template>
    <div ref="dom"></div>
</template>

<script>
import echarts from 'echarts'
import { on, off } from '@/libs/tools'
export default {
  data () {
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
    function toPercent(point){
    var str=Number(point*100).toFixed(2);
    //str+="%";
    return str;}
    this.loctest1=window.localStorage.getItem('tableTitle')
    var all_data=eval(this.loctest1)
    
    
    //分箱函数
    var split_bin=function(x){
      if(x<10){return '0~9'}
      else if(x<20){return '10~19'}
      else if(x<30){return '20~29'}
      else if(x<40){return '30~39'}
      else if (x>=40){return '40+'}
      else{}
    } 

    //确定月份
    var month_detail=[]
    for (var i=0,len=all_data.length;i<len;i++){
      if (month_detail.indexOf(all_data[i]['月份']) === -1) {
                    month_detail.push(all_data[i]['月份']);
                }
    }
    var current_month=month_detail[month_detail.length-1]

    var colorList = [
                    "#FE2E2E", "#2d8cf0","#04B45F","#2d8cf0","#19be6b","#9A66E4","#ed3f14"
                    ]    
    var legend_show=['近三个月机构查询','近六个月机构查询','历史机构查询']
    var legend_list=['近三个月机构查询次数','近六个月机构查询次数','机构查询总次数']
    var bin_sort=["0~9","10~19","20~29", "30~39", "40+"]


    var series_list=[]
    for(var i=0,len=legend_list.length;i<len;i++){
        var ret= {name: legend_show[i],type: 'bar','color':colorList[i],'label':{'show':true,'position': 'top','formatter': '{c}'}}
        var data1={'0~9':0,'10~19':0,'20~29':0,'30~39':0,'40+':0}
        for (var j=0,len1=all_data.length;j<len1;j++){
            if(all_data[j]['月份']==current_month){
                var cur_hit=split_bin(all_data[j][legend_list[i]])
                //console.log(cur_hit,all_data[j][legend_list[i]])
                data1[cur_hit]=data1[cur_hit]+1
            }
        }        
        ret['data']=[data1['0~9'],data1['10~19'],data1['20~29'],data1['30~39'],data1['40+']]
        series_list.push(ret)  
    }
    //console.log(series_list)
    //console.log(all_data1)
    // var current_month_num=0,last_month_num=0,history_num=0;
    // var current_month_force=0,current_month_dns=0,current_month_mailser=0,current_monthseo=0;
    // var last_month_force=0,last_month_dns=0,last_month_mailser=0,last_monthseo=0;
    // var history_force=0,history_dns=0,history_mailser=0,history_seo=0;


    // for (var i=0,len=all_data1.length;i<len;i++){
    //   if(all_data1[i]['月份']=='本月'){
    //     current_month_num=current_month_num+1
    //     if(all_data1[i]['命中暴力破解']==1){current_month_force=current_month_force+1}
    //     if(all_data1[i]['命中DNS服务器']==1){current_month_dns=current_month_dns+1}
    //     if(all_data1[i]['命中邮件服务器']==1){current_month_mailser=current_month_mailser+1}
    //     if(all_data1[i]['命中seo']==1){current_monthseo=current_monthseo+1}

    //   } 
    //   else if (all_data1[i]['月份']=='上月'){
    //     last_month_num=last_month_num+1
    //     if(all_data1[i]['命中暴力破解']==1){last_month_force=last_month_force+1}
    //     if(all_data1[i]['命中DNS服务器']==1){last_month_dns=last_month_dns+1}
    //     if(all_data1[i]['命中邮件服务器']==1){last_month_mailser=last_month_mailser+1}        
    //     if(all_data1[i]['命中seo']==1){last_monthseo=last_monthseo+1}
    //   }
    //   else if (all_data1[i]['月份']=='历史平均'){
    //     history_num=history_num+1
    //     if(all_data1[i]['命中暴力破解']==1){history_force=history_force+1}
    //     if(all_data1[i]['命中DNS服务器']==1){history_dns=history_dns+1}
    //     if(all_data1[i]['命中邮件服务器']==1){history_mailser=history_mailser+1}        
    //     if(all_data1[i]['命中seo']==1){history_seo=history_seo+1}
    //   }
    //   else{}
    // }

//    console.log(current_month_num,last_month_num,history_num)
//    console.log(current_month_force,current_month_dns,current_month_mailser,current_monthseo)
//    console.log(last_month_force,last_month_dns,last_month_mailser,last_monthseo)
//    console.log(history_force,history_dns,history_mailser,history_seo)
      // console.log(toPercent(current_month_force/current_month_num))


    const option = {
    backgroundColor: "rgba(0,0,0,0)",
    title:{text: '本月各机构查询次数分箱对比',x: 'center', textStyle: {
            "color": "#516b91"
        }},
    legend: {
              orient: 'horizontal',
              right: 'right',
              data: legend_show      
            },
    tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} '
    },
      xAxis: [
        {
          data: bin_sort,
          splitLine:{show: false},
          name:'查询次数分箱',
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
            fontSize:8,
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
