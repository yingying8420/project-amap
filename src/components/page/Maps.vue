<template>
  <div class="maps-warpper">
    <!-- 地图框架 -->
    <el-amap
      class="maps-conts"
      :vid="'amapDemo'"
      :zoom="zoom"
      :amap-manager="amapManager"
      :center="center"
      :mapStyle="mapStyle"
    >
      <!-- 地图点标记 -->
      <el-amap-marker
        v-for="(marker, index) in markers"
        :key="index"
        :position="marker.position"
        :events="marker.events"
      ></el-amap-marker>

      <!-- 信息窗体 -->
      <el-amap-info-window
        v-if="window"
        :position="window.position"
        :content="window.content"
        :visible="window.visible"
        :events="window.events"
        :anchor="window.anchor"
      ></el-amap-info-window>

      <!-- 组件 -->
      <el-button type="primary" :v-show="false">主要按钮</el-button>
    </el-amap>
  </div>
</template>

<script>
// 导入vue-amap包
import Vue from "vue";
import VueAMap from "vue-amap";
let amapManager = new VueAMap.AMapManager();

Vue.use(VueAMap);

export default {
  data() {
    // 初始化地图属性
    return {
      amapManager,
      zoom: 12,
      center: [116.405285, 39.904989],
      mapStyle: "amap://styles/16bf8838b8aaa63eef99fe12a787ca00",
      markers: [],
      windows: [],
      window: ""
    };
  },
  created() {
    //初始化地图
    VueAMap.initAMapApiLoader({
      key: "6172b79ae8e4b3d99e1ff9b9158c40c8",
      v: "1.4.13"
    });
    let markers = [];
    let windows = [];
    let num = 5;
    let me = this;
    

    // 循环点坐标
    for (let i = 0; i < num; i++) {
      markers.push({
        position: [116.405285 + i * 0.01, 39.904989 + i * 0.01],
        events: {
          click(e) {
            // console.log(VueAMap.AmapInfoWindow.mixins[0].methods.$$getInstance())
            me.windows.forEach(window => {
              window.visible = false;
            });

            me.window = me.windows[i];

            me.$nextTick(() => {
              me.window.visible = true;
            });
          }
        }
      });

      // 渲染信息窗体
      windows.push({
        position: [116.405285 + i * 0.01, 39.904989 + i * 0.01 + 0.005],
        content: `<div>
                    <span>这是第${i}个坐标点</span>
                    <p>经度：${116.405285 + i * 0.01}</p>
                    <p>纬度：${39.904989 + i * 0.01}</p>
                  </div>`,
        // template: `<el-button type="primary">主要按钮</el-button>`,
        visible: false
      });
    }
    this.markers = markers;
    this.windows = windows;
  }
};
</script>

<style>
.maps-warpper {
  width: 100%;
  height: 100%;
  position: relative;
}
</style>