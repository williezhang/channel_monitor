<template>
  <div class="header-bar">
  <!-- <sider-trigger :collapsed="collapsed" icon="md-menu" @on-change="handleCollpasedChange"></sider-trigger> -->
    <custom-bread-crumb show-icon style="margin-left: 30px;" :list="breadCrumbList"></custom-bread-crumb>
    <!-- <div><p>您选择</p></div> -->
    <div class="custom-content-con">
      <slot></slot>
    </div>
          
          <!-- <select id= "sel" style="width:100px;height:40px;position: relative;left: 18%;">
          <option value="产品1">产品1</option>
          <option value="产品2" selected="selected">产品2</option>
          <option value="产品3">产品3</option>
          </select>   

          <select id= "sel" style="width:100px;height:40px;position: relative;left: 28%;">
          <option value="渠道1">渠道1</option>
          <option value="渠道2" selected="selected">渠道2</option>
          <option value="渠道3">渠道3</option>
          </select>   -->

     <Dropdown style="position: relative;left: 60%;">
        <Button type="primary">
            请选择产品
            <Icon type="ios-arrow-down"></Icon>
        </Button>
        <DropdownMenu slot="list">
            <DropdownItem>产品1</DropdownItem>
            <DropdownItem>产品2</DropdownItem>
            <DropdownItem>产品3</DropdownItem>
        </DropdownMenu>
    </Dropdown>
    <Dropdown style="position: relative;left: 65%;">
        <Button type="primary">
            请选择渠道
            <Icon type="ios-arrow-down"></Icon>
        </Button>
        <DropdownMenu slot="list">
            <DropdownItem>渠道1</DropdownItem>
            <DropdownItem>渠道2</DropdownItem>
            <DropdownItem>渠道3</DropdownItem>
        </DropdownMenu>
    </Dropdown>   

  <Dropdown style="position: relative;left: 10%;">
    <h1 style="text-align:center">渠道进件监控报表</h1>
    <!-- <p></p> -->
    </Dropdown>
  </div>
</template>
<script>
import siderTrigger from './sider-trigger'
import customBreadCrumb from './custom-bread-crumb'
import './header-bar.less'
export default {
  name: 'HeaderBar',
  components: {
    siderTrigger,
    customBreadCrumb
  },
  props: {
    collapsed: Boolean
  },
  computed: {
    breadCrumbList () {
      return this.$store.state.app.breadCrumbList
    }
  },
  methods: {
    handleCollpasedChange (state) {
      this.$emit('on-coll-change', state)
    }
  }
}
</script>
