<template>
  <div class="params-msg">
    <head-top headTitle = "参数信息" goBack = "true"></head-top>
    <div class = "section scroll-box">
        <div class="con-list-sm padding">
            <light-cell v-for = "(item,index) in msgList" :key="index" :lfCon = "item.title" :rtCon = "item.con"></light-cell>
        </div>
        <div class="con-list-sm padding">
            <light-cell lfCon = "节点参数"> <distribute slot="other"></distribute> </light-cell>
            <light-cell v-for = "(item,index) in portOptions" msgIcon = "true" :key="index" :lfCon = "item.lj" :mdCon="item.wl" :blue = "item.state == 1?true:false" :rtCon = "item.state == 1?'有效':'无效'"></light-cell>   
        </div>
        <div class="con-list-sm padding">
            <light-cell lfCon = "分组参数"> <distribute slot="other"></distribute> </light-cell>
            <light-cell lfCon = "智慧灯杆" mdCon="组号:2" msgIcon="true"></light-cell>
        </div>
        <div class="con-list-sm padding">
            <light-cell lfCon = "经纬度参数"> <distribute slot="other"></distribute> </light-cell>
            <light-cell lfCon = "经度" msgIcon="true"><ipt-apen v-model = "lng" slot="other"></ipt-apen></light-cell>
            <light-cell lfCon = "纬度" msgIcon="true"><ipt-apen v-model = "lat" slot="other"></ipt-apen></light-cell>
        </div>
        <light-offset title = "开灯偏移量" :ParentValue = "onValue" @getValue = 'getOnValue' :ParentOffset = "onOffset" @getOffset = "getOnOffset"></light-offset>
        <light-offset title = "关灯偏移量" :ParentValue = "offValue" @getValue = 'getOffValue' :ParentOffset = "offOffset" @getOffset = "getOffOffset"></light-offset>
        <distribute all = "true"></distribute>
    </div>
  </div>
</template>

<script>
import headTop from '@/components/header/mainHeader.vue'
import lightCell from '@/components/common/lightCell'
import iptApen from '@/components/common/iptApen'
import lightOffset from '@/components/common/lightOffset'
import distribute from '@/components/common/distribute'
import Bus from "@/config/bus.js";

export default {
  data () {
    return {
        msgList:[
            {"title":"灯杆名称","con":"智芯OUN"},
            {"title":"灯杆号","con":"21212345123213"},
        ],//展示数据
        portOptions:[{lj:'逻辑节点:0',wl:'物理节点: 0',state:'1' },{lj:'逻辑节点:1',wl:'物理节点: 1',state:'1' },{lj:'逻辑节点:3 ',wl:'物理节点: 2',state:'false'}],//逻辑节点
        lng:154984238,   //经度
        lat:586486481,  //纬度
        onValue:'滞后',   //开灯偏移量radio
        onOffset:22,    //开灯偏移量range
        offValue:'提前',   //关灯偏移量radio
        offOffset:20,    //关灯偏移量range
        pole:''// 灯杆信息
    }
  },
  methods:{
    getOnValue(value){
        this.onValue = value
    },
    getOnOffset(value){
        this.onOffset = value
    },
    getOffValue(value){
        this.offValue = value
    },
    getOffOffset(value){
        this.offOffset = value
    },
    setPoleMsg(pole){  //获取灯杆信息
        this.msgList[0].con = pole.dgmc
        this.msgList[1].con = pole.dgh
    },
  },
  components:{
      headTop,
      lightCell,
      iptApen,
      lightOffset,
      distribute
  },
  created() {
    this.pole = this.$route.query.pole
    this.setPoleMsg(this.pole)
  },
  beforeDestroy() {
      Bus.$emit('paramMsg',this.pole)
      Bus.$off('paramMsg')
  },
}
</script>

<style scoped lang = "scss">
@import '@/style/mixin.scss';
.params-msg{
    @include wh(100%,100%)
}
</style>
