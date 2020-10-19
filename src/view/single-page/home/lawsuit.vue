<template>
    <div id='law' ref="dom">
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


    var myecharts = echarts.init(document.getElementById("law"));
     var option={ 
        title:{
            text: '本月命中涉诉通人员详情',
            x: 'center',
             textStyle: {
            "color": "#516b91"}
            },
        legend: {                       //图例
              show:false,                   //是否显示图例
              orient: 'vertical',           //布局方向
              x: 'left',                    //图例相对位置
              data:['3','4','1','2'] //图例内容
            },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        series: [
        {
            type: 'pie',
            selectedMode: 'single',
            radius:  '50%',
            name:'',
            label: {
                position: 'inner'
            },
            labelLine: {
                show: false
            },
            data: [
                {value: 40, name: '命中被执行人'},
                {value: 24, name: '未命中被执行人'},
            ],
            color:["#C1232B", "#2d8cf0"]
        },
        {
            type: 'pie',
            name:'',
            radius: ['60%', '75%'],
            data: [
                {value: 25, name: '失信执行人'},
                {value: 15, name: '非失信执行人'},
            ],
            color:["#19be6b", "#ff9900"]
        }
        ],
         
 
     };
    myecharts.setOption(option);

    //嵌套饼图联动
    function eConsole(param) {
       if (typeof param.seriesIndex != 'undefined') {
         if (param.type == 'click') {       //判断事件类型：点击
            //写法一：获取饼图散区个数,按照图例获取 
           var peiLenght= option.legend.data.length;
           //写法二：获取饼图散区个数,按照series中data获取
           //var peiLenght= option.series[0].data.length;

           for(var i=0;i<peiLenght;i++){
             //seriesIndex==0：选择内环饼图；param.dataIndex==i：散区
             if(param.seriesIndex==0&&param.dataIndex==i){  
                 console.log(i)
               if(i==0){
                 var option_1 = myecharts.getOption();
                // option_1.series[1].color=['#205E3F','#16DA69','red'],
                 option_1.series[1].data=[
                    {value: 25, name: '失信执行人'},
                    {value: 15, name: '非失信执行人'},
                ];
                 option_1.series[1].color=["#19be6b", "#ff9900"]; 
                 console.log(option_1)
                 myecharts.setOption(option_1);
               }else if(i==1){
                 var option_2 = myecharts.getOption();
                // option_2.series[1].color=['#16DA69','#205E3F','red'],
                 option_2.series[1].data=[
                    {value:24,name:'未命中被执行人'},
                 ];
                 option_2.series[1].color=["#19be6b"];                
                 console.log(option_2)
                 myecharts.setOption(option_2);
               }
             }
           }
         }
       }
     }
    myecharts.on("click", eConsole);

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

