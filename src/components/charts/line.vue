<template>
  <div ref="dom" class="charts chart-bar"></div>
</template>

<script>
import echarts from 'echarts'
import tdTheme from './theme.json'
import { on, off } from '@/libs/tools'
echarts.registerTheme('tdTheme', tdTheme)
export default {
  name: 'ChartLine',
  props: {
    value: Object,
    text: String,
    subtext: String
  },
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
    this.$nextTick(() => {


      let legend = this.value.map(_ => _.name)
      let xAxisData = this.value.map(_ => _.name)
     // console.log('xAxisData',xAxisData)

      let seriesData = Object.values(this.value)


     // console.log('seriesData',seriesData)


      let option = {
        title: {
          text: this.text,
          subtext: this.subtext,
          x: 'center'
        },
        tooltip: {
          trigger: 'axis',
          formatter: '{a} <br/>{b}: {c}% '
        },
        legend: {
          data: legend
        },
        xAxis: {
          type: 'category',
          splitLine:{//去除网格线
            show:false
          },
          data: xAxisData
        },
        yAxis: {
          type: 'value',
          splitLine:{//去除网格线
            show:false
          },
        },
        series: [{
          name:'',
          data: seriesData,
          type: 'line',
          itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              },
              normal:{
                  label: {
                        show: true,
                        position: 'top',
                        formatter: '{c}%'　　　　//这是关键，在需要的地方加上就行了
                  }
              }
            }
        }]
      }
      this.dom = echarts.init(this.$refs.dom, 'tdTheme')
      this.dom.setOption(option)
      on(window, 'resize', this.resize)
    })
  },
  beforeDestroy () {
    off(window, 'resize', this.resize)
  }
}
</script>
