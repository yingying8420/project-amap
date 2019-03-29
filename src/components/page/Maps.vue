<template>
  <div class="maps-warpper">
    <el-amap class="maps-conts" :vid="'amap-vue'" :zoom="zoom" :center="center" :mapStyle="mapStyle">
      <!-- 地图打点 -->
      
        <el-amap-marker v-for="marker in markers" :position="marker.position" :events="marker.events"></el-amap-marker>
      <el-popover placement="right" title="标题" width="200" trigger="click" content="这是一段内容,这是一段内容,这是一段内容,这是一段内容。"></el-popover>
    </el-amap>

    <el-row class="fix-print-btn">
      <el-button type="success" @click="printBtn">打印地图，其他的不打</el-button>
    </el-row> 
    

    <!-- <el-table v-show="tables" :data="tableData" style="width: 300px;height: 200px;position: absolute; right: 10px; top: 10px;border: 1px solid #ccc;">
      <el-table-column prop="date" label="日期" width="100"></el-table-column>
      <el-table-column prop="name" label="姓名" width="100"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
    </el-table>-->
    <!-- 信息提示窗体 -->
    <el-card
      class="box-card"
      v-show="tip_card"
      style="position: absolute;right: 10px;top: 10px;width: 20%;"
    >
      <div slot="header" class="clear-fix">
        <span>标 题</span>
        <el-button
          icon="el-icon-close"
          circle
          @click="this_close"
          style="float: right; padding: 5px"
        ></el-button>
      </div>
      <div v-for="o in 5" :key="o" class="text-list">{{'列表内容 ' + o }}</div>
    </el-card>

    <!-- 图例 -->
    <div class="legend-area">
      <span class="legend-title">图 1 例</span>
      <br>
      <el-checkbox
        :indeterminate="isIndeterminate"
        v-model="checkAll"
        @change="handleCheckAllChange"
      style="margin: 10px;">全选</el-checkbox>
      <el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange" >
        <el-checkbox v-for="city in cities" :label="city" :key="city" style="margin: 10px;">{{city}}
          <i class="el-icon-edit"></i>
        </el-checkbox>
      </el-checkbox-group>
    </div>
  </div>
</template>

<script>
const cityOptions = ["蓝色", "红色", "绿色", "紫色"];
export default {
  data() {
    return {
      zoom: 12,
      center: [116.405285, 39.904989],
      mapStyle: 'amap://styles/16bf8838b8aaa63eef99fe12a787ca00',
      // plugin: [{ pName: "Scale" }, { pName: "ToolBar" }],
      markers: [],
      // windows: [],
      // window: "",
      tip_card: false,

      checkAll: false,
      checkedCities: ['蓝色'],
      cities: cityOptions,
      isIndeterminate: true
      // tableData: [
      //   {
      //     date: "2019-03-27",
      //     name: "zhang san",
      //     address: "beijing"
      //   },
      //   {
      //     date: "2019-03-28",
      //     name: "li si",
      //     address: "shanghai"
      //   },
      //   {
      //     date: "2019-03-29",
      //     name: "wang wu",
      //     address: "nanjing"
      //   }
      // ]
      // tipVisible: true
    };
  },
  methods: {
    this_close() {
      this.tip_card = false;
    }, 
    handleCheckAllChange(val) {
      this.checkedCities = val ? cityOptions : [];
      this.isIndeterminate = false;
    },
    handleCheckedCitiesChange(value) {
      let checkedCount = value.length;
      this.checkAll = checkedCount === this.cities.length;
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.cities.length;
    },
    printBtn(){ // 打印地图
      let header = document.getElementsByClassName('header')[0]
      let sidebar = document.getElementsByClassName('sidebar')[0]
      let tags = document.getElementsByClassName('tags')[0]
      let sidebarMenu = document.getElementsByClassName('sidebar-el-menu')[0]
      let legendArea = document.getElementsByClassName('legend-area')[0]
      // let contentBox = document.getElementsByClassName('content-box')[0]

      document.getElementsByClassName('content-box')[0].style.top = '0'
      document.getElementsByClassName('content-box')[0].style.left = '0'
      document.getElementsByClassName('content-box')[0].style.width = '100%'

      header.classList.add('print-hide-css')
      sidebar.classList.add('print-hide-css')
      tags.classList.add('print-hide-css')
      sidebarMenu.classList.add('print-hide-css')
      legendArea.classList.add('print-hide-css')



      window.print()
      window.location.reload()

    }
  },
  created() {
    let markers = [];
    // let windows = [];
    let num = 5;
    let self = this;

    for (let i = 0; i < num; i++) {
      markers.push({
        position: [116.405285 + i * 0.01, 39.904989 + i * 0.01],
        events: {
          click(e) {
            console.log(self);
            // if (self.tip_card = false) {
            self.tip_card = true;
            // } else {
            // self.tip_card = false
            // }
          }
        }
      });
    }

    this.markers = markers;
  }
};
</script>

<style>
.maps-warpper {
  width: 100%;
  height: 100%;
  position: relative;
}
.text-list {
  /* font-size: 14px; */
  height: 24px;
  line-height: 24px;
}
.legend-area {
  position: absolute;
  background: #ffffff;
  left: 10px;
  top: 10px;
  width: 10%;
  height: 80%;
  z-index: 1000;
  box-shadow: rgb(172, 172, 172) 3px 3px 10px;
  border-radius: 5px;
}
.legend-area span.legend-title {
  font-size: 14px;
  color: #666;
  font-weight: bold;
  display: block;
  padding: 10px;
  border-bottom: 1px solid #eee;
}
.fix-print-btn{
  position: absolute;
  top: 10px;
  left: 45%;
}
/* 添加一个隐藏的类 */
.print-hide-css{
  display: none;
}
</style>