<template>
<div>

  <Row :gutter="20">
      <i-col :xs="12" :md="8" :lg="6" v-for="(infor, i) in inforCardData" :key="`infor-${i}`" style="height: 120px;padding-bottom: 10px;">
        <infor-card shadow :color="infor.color" :icon="infor.icon" :icon-size="36">
        <!--   <count-to :end="infor.count" count-class="count-style"/>-->
          <p style="font-size: 35px;"> {{infor.count}}</p>
          <p>{{ infor.title}}</p>
        </infor-card>
      </i-col>
    </Row>

    <el-divider></el-divider>
      <Alert>
          <h2 style="text-align:center">基本信息</h2>
      </Alert>
    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
         <mymap style="height: 300px;"/>
        </Card>
      </i-col>

      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-circle style="height: 300px;" :value="sex" text="本月进件男女对比"/>
        </Card>
      </i-col>

      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-circle style="height: 300px;" :value="age" text="本月进件年龄对比"/>
        </Card>
      </i-col>
    </Row>

    <el-divider></el-divider>
       <Alert>
          <h2 style="text-align:center">信息核验</h2>
          <ul>
            <li style="font-size: 15px !important;margin: 10px 0 0 10px;">【真实身份】客户比例近半年表现有大幅波动，6个月的极差为35.17%，本月呈下降波动，值得重点关注</li>
            <li style="font-size: 15px !important;margin: 10px 0 0 10px">【三要素一致】客户比例近半年表现相对平缓，6个月的平均值为80.28%，表现稳定</li>
          </ul>
      </Alert>
    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
      <Card shadow>
         <chart-line style="height: 300px;" :value="ssncheck" text="各月真实身份客户对比"/>
      </Card>
      </i-col>
      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-line style="height: 300px;" :value="mobilecheck" text="各月三要素一致客户对比"/>
        </Card>
      </i-col>
    </Row>

    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-pie style="height: 300px;" :value="mobilestatus" text="本月手机号状态对比"/>
        </Card>
      </i-col>

     <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-pie style="height: 300px;" :value="onlinetime" text="本月手机号在网时长对比"/>
        </Card>
      </i-col>
    </Row>


    <el-divider></el-divider>
    <Alert>
        <h2 style="text-align:center">反欺诈</h2>
          <ul>
            <li style="font-size: 15px !important;margin: 10px 0 0 10px;">【命中反欺诈】客户比例近半年表现大幅波动，6个月的极差为9.33%，本月呈下降波动，值得重点关注</li>
          </ul>    
     </Alert>
    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="24" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-line style="height: 300px;" :value="isfraud" text="各月命中反欺诈对比"/>
        </Card>
      </i-col>
    </Row>

    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="24" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-bar style="height: 300px;" :value="fraud_detail2" text="本月手机号反欺诈详情"/>
        </Card>
      </i-col>
    </Row>



    <el-divider></el-divider>
      <Alert>
        <h2 style="text-align:center">网贷风险</h2>
        <ul>
          <li style="font-size: 15px !important;margin: 10px 0 0 10px;">【命中网贷】客户比例近半年表现相对平缓，6个月的平均值为2.26%，表现稳定</li>
        </ul>
       </Alert>
    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="24" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-line style="height: 300px;" :value="iswebloan" text="各月命中网贷风险对比"/>
        </Card>
      </i-col>
    </Row>

    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-pie style="height: 300px;" :value="loantype" text="本月贷款类型对比"/>
        </Card>
      </i-col>

      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-pie style="height: 300px;" :value="overdueday" text="本月逾期时长对比"/>
        </Card>
      </i-col>

      <i-col :md="24" :lg="8" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-pie style="height: 300px;" :value="overdueamount" text="本月逾期欠款对比"/>
        </Card>
      </i-col>

    </Row>

    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="24" style="margin-bottom: 20px;">
        <Card shadow>
         <ishitorg style="height: 300px;"/>
        </Card>
      </i-col>
    </Row>

    <Row :gutter="20" style="margin-top: 10px;">

      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
         <ishitevery style="height: 300px;"/>
        </Card>
      </i-col>

      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
         <example style="height: 300px;"/>
        </Card>
      </i-col>
    </Row>


  <el-divider></el-divider>
    <Alert>
        <h2 style="text-align:center">涉诉通</h2>
          <ul>
            <li style="font-size: 15px !important;margin: 10px 0 0 10px;">【命中涉诉通】客户比例近半年有大幅波动，6个月的极差为9.34%，本月呈下降波动，值得重点关注</li>
          </ul>  
    </Alert>
    <Row :gutter="20" style="margin-top: 10px;">
      <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
          <chart-line style="height: 300px;" :value="islawsuit" text="各月命中涉诉通对比"/>
        </Card>
      </i-col>

        <i-col :md="24" :lg="12" style="margin-bottom: 20px;">
        <Card shadow>
         <lawsuit style="height: 300px;"/>
        </Card>
      </i-col>
    </Row>



  <!--     <Row :gutter="20" style="margin-top: 10px;">
      <Card shadow>
        <example style="height: 310px;"/>
      </Card>
    </Row>

   <Row :gutter="20" style="margin-top: 10px;">
      <Card shadow>
        <mybar style="height: 310px;"/>
      </Card>
    </Row> -->
 <!--   <vue-plotly :data="plotdata"/> -->


</div>
</template>
<!--
<style>
	page {
		background:red;
	}
</style>


<style>
    .special{font-size: 30px !important;}
</style>
-->

<script>
import InforCard from '_c/info-card'
import CountTo from '_c/count-to'
import { ChartPie, ChartBar,ChartLine,ChartCircle} from '_c/charts'
import Example from './querysearch.vue'
import Mybar from './mybar.vue'
import  Ishitorg from './ishitorg.vue'
import  Ishitevery from './ishitevery.vue'
import Mymap from './mymap.vue'
import Lawsuit from './lawsuit.vue'

import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

//Vue.use();
Vue.use(ElementUI);

import VueDarkMode from "@growthbunker/vuedarkmode";

Vue.use(VueDarkMode);
//import VuePlotly from '@statnett/vue-plotly'
/*import Vue from 'vue';
import HistogramSlider from 'vue-histogram-slider';
import 'vue-histogram-slider/dist/histogram-slider.css';
 
Vue.component(HistogramSlider.name, HistogramSlider);*/


export default {
  name: 'home',
  components: {
    InforCard,
    CountTo,
    ChartPie,
    ChartBar, 
    ChartLine,
    ChartCircle,   
    Example,
    Mybar,
    Ishitorg,
    Ishitevery,
    Mymap,
    Lawsuit
  },
  data () {
    return {
      loctest:Object,
    //本月进件、通过率、放款率、总金额
    //涉诉信息：命中涉诉信息、命中被执行信息、命中失信被执行信息
    inforCardData: [
        { title: '本月进件客户总数', icon: 'md-person-add', count: '342', color: '#2d8cf0' },
        { title: '本月申请通过率', icon: 'md-map', count: '78.4%', color: '#9A66E4' },
        { title: '本月放款率', icon: 'md-locate', count: '58.1%', color: '#19be6b' },
        { title: '本月放款总金额', icon: 'md-map', count: '5,000,000', color: '#9A66E4' }
      ],

    sex:Object,
    age:Object,
    ssncheck: Object,
    mobilecheck:Object, 


     isfraud:Object,
     fraud_detail:Object,
     fraud_detail2:Object,
			


     iswebloan:Object,
     islawsuit:Object,
 /*   ssncheck:Object,

    mobilecheck: [
        { value: '897', name: '4月' },
        { value: '650', name: '5月' },
        { value: '372', name: '6月' },
        { value: '472', name: '7月' },
        { value: '392', name: '8月' },
        { value: '312', name: '9月' },
      ],*/
    mobilestatus:Object,
    onlinetime:Object,
    loantype:Object,
    overdueday:Object,
    overdueamount:Object
    }
  },
  mounted () {

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


    var age_bin=function(x){
      if(x<=35){return '青年'}
      else if(x<=60){return '中年'}
      else {return '老年'}
    }


    //x为字段，y为选择的月份，返回当月某个字段的枚举值，用来画饼图    
    var pick_age=function(x,y){
      var example_num={}
      for (var i=0,len=all_data.length;i<len;i++){
        if (all_data[i]['月份']==y) {
          var agebin=age_bin(all_data[i][x])
          if(example_num.hasOwnProperty(agebin)){example_num[agebin]=example_num[agebin]+1}
          else{example_num[agebin]=1}
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


    //保留四位小数
    var to4float=function(number){
      number = String(number*100).replace(/^(.*\..{2}).*$/,"$1");
      return number
    }


    //计算百分比
    var cal_percent=function(x,y){
      var total_num=0
      var hit_num=0
      for (var i=0,len=all_data.length;i<len;i++){
        if (all_data[i]['月份']==y) {
          total_num=total_num+1
          if(all_data[i][x]==1){hit_num=hit_num+=1}
        }
      } 
      //return  total_num,hit_num,(hit_num/total_num).toFixed(4)*100  
      return  to4float(hit_num/total_num)
 }


    //确定月份
    var month_detail=[]
    var month_count={}
    for (var i=0,len=all_data.length;i<len;i++){
      //console.log(all_data[i]['在网时长(使用时间分数)'],typeof(all_data[i]['在网时长(使用时间分数)']))
      if (month_detail.indexOf(all_data[i]['月份']) === -1) {
                    month_detail.push(all_data[i]['月份']);
                }
      if(month_count.hasOwnProperty(all_data[i]['月份'])){month_count[all_data[i]['月份']]=month_count[all_data[i]['月份']]+1}
      else{month_count[all_data[i]['月份']]=1}
    }
//    console.log(month_detail,month_count)

    var cal_percent_allmonth=function(x,y){
      var allmonth={}
      for(var i=0,len=month_detail.length;i<len;i++){
        allmonth[month_detail[i]]=0
      }
      for(var i=0,len=all_data.length;i<len;i++){
          if(all_data[i][x]==y){
            allmonth[all_data[i]['月份']]=allmonth[all_data[i]['月份']]+1
            }
      }
      var ret=[]
      for(var i=0,len=month_detail.length;i<len;i++){
        ret.push({'name':month_detail[i],'value':to4float(allmonth[month_detail[i]]/month_count[month_detail[i]])})
      } 
      console.log(x,y,allmonth,ret)

      return ret
    }


    //四条曲线
    this.ssncheck=cal_percent_allmonth('是否真实身份','真实身份')
    this.mobilecheck=cal_percent_allmonth('手机验证结果','三要素一致')
    this.iswebloan=cal_percent_allmonth('是否命中网贷风险资讯','命中')
    this.isfraud=cal_percent_allmonth('是否命中反欺诈',1)
    this.islawsuit=cal_percent_allmonth('是否命中涉诉通',1)
    //console.log(this.islawsuit)
//     var ssn_dic={},mobilecheck_dic={},iswebloan_dic={},isfraud_dic={}
//     for (var i=0,len=month_detail.length;i<len;i++){
//         ssn_dic[month_detail[i]]=0
//         mobilecheck_dic[month_detail[i]]=0
//         iswebloan_dic[month_detail[i]]=0
//         isfraud_dic[month_detail[i]]=0
//     }

//      //构造曲线需要用到的字典,用到下面四个字段
//     for (var i=0,len=all_data.length;i<len;i++){
//       var every_month= all_data[i]['月份']
//       if(all_data[i]['是否真实身份']=="真实身份"){
//         ssn_dic[every_month]=ssn_dic[every_month]+1
//         }
//       if(all_data[i]['手机验证结果']=="三要素一致"){
//         mobilecheck_dic[every_month]=mobilecheck_dic[every_month]+1
//         }      
//       if(all_data[i]['是否命中网贷风险资讯']=="命中"){
//         iswebloan_dic[every_month]=iswebloan_dic[every_month]+1
//         }
//       if(all_data[i]['是否命中反欺诈']==1){
//         isfraud_dic[every_month]=isfraud_dic[every_month]+1
//         }        
//     }
//    // console.log(ssn_dic,mobilecheck_dic,iswebloan_dic,isfraud_dic)
//     //真实身份统计
//     var ssncheck_show=[]
//     for(var key in ssn_dic){                
//       ssncheck_show.push({value:ssn_dic[key],name:key})}     
//     this.ssncheck=ssncheck_show


//     //手机验证结果统计,三要素
//     var mobilecheck_show=[]
//     for(var key in mobilecheck_dic){                
//       mobilecheck_show.push({value:mobilecheck_dic[key],name:key})}     
//     this.mobilecheck=mobilecheck_show    

//     //命中反欺诈
//     var isfraud_show=[]
//     for(var key in isfraud_dic){                
//       isfraud_show.push({value:isfraud_dic[key],name:key})}     
//     this.isfraud=isfraud_show       
//  //   console.log(isfraud_show)
//     //命中网贷风险
//     var iswebloan_show=[]
//     for(var key in iswebloan_dic){                
//       iswebloan_show.push({value:iswebloan_dic[key],name:key})}     
//     this.iswebloan=iswebloan_show


   // console.log(this.ssncheck,this.mobilecheck,this.isfraud,this.iswebloan)
    var current_month=month_detail[month_detail.length-1]
    var sex_example=pick("性别",current_month)
    this.sex=sex_example


    //本月手机号状态-饼图
    var mobilestatus_example=pick("手机号状态",current_month)
    this.mobilestatus=mobilestatus_example

    //本月在网时长-饼图
    var onlinetime_example=pick("在网时长(使用时间分数)",current_month)
  //  console.log(onlinetime_example)
    this.onlinetime=onlinetime_example


    var age_example=pick_age('年龄',current_month)
    this.age=age_example





    //命中反欺诈-柱状图
    var fraud1_name=["命中暴力破解","命中DNS服务器","命中邮件服务器","命中seo","命中组织出口","命中爬虫","命中代理服务器","命中web服务器","命中vpn服务器"]
    var fraud1_show=[]
    for(var i=0,len=fraud1_name.length;i<len;i++){ 
      fraud1_show.push({value:cal_percent(fraud1_name[i],current_month),name:fraud1_name[i]})
    }  
    this.fraud_detail=fraud1_show


    //命中反欺诈-柱状图
    var fraud2_name=["命中虚假手机号","命中通信小号","命中标记号码","是否虚拟手机号"]
    var fraud2_show=[]
    for(var i=0,len=fraud2_name.length;i<len;i++){ 
      fraud2_show.push({value:cal_percent(fraud2_name[i],current_month),name:fraud2_name[i]})
    }  
    this.fraud_detail2=fraud2_show
   // [machdforce,machddns,machdmail,machdseo,machdorg,machdcraw,machdproxy,machdweb,machdvpn]


    //本月贷款类型-饼图
    var loantype_example=pick("贷款类型",current_month)
  //  console.log(onlinetime_example)
    this.loantype=loantype_example

    //逾期天数
    var overdueday_example=pick("逾期时长",current_month)
  //  console.log(onlinetime_example)
    this.overdueday=overdueday_example
    
  //欠款金额
     var overdueamount_example=pick("欠款金额",current_month)
  //  console.log(current_month,overdueamount_example)
    this.overdueamount=overdueamount_example 


  },
  beforeCreate () {
    document.querySelector('body').setAttribute('style', 'background-color:#FE2E2E')
    },
  beforeDestroy () {
    document.querySelector('body').removeAttribute('style','')
  }


}

//


</script>

<style lang="less">
.count-style{
  font-size: 50px;

}
</style>
