<template>
  <div class="my_main_content">
    <!-- 图表组件 -->
    <middleware ref="middleware" :settingConfig="settingConfig" @rowClick="rowClick"></middleware>
    <!-- 地图组件 -->
    <!-- <myMap ref="myMaps"></myMap> -->
    <!-- 页面配置 -->
    <div class="hoverMenu">
      <div class="box">
        <div class="top theme-bg-color">
          <i
            title="页面配置"
            class="iconfont iconkuangjiashezhi"
          ></i>
        </div>
        <ul class="theme-box-shadow">
          <li
            @click="rightDrawerShow('assembly')"
            :class="{ 'theme-color': chooseType == 1 }"
            @mouseout="chooseType = null"
            @mouseover="chooseType = 1"
          >
            页面
          </li>
        </ul>
      </div>
    </div>
    <el-drawer
      :visible.sync="settingDrawer"
      :title="rightDrawerType == 'menu' ? '菜单配置' : '组件新增'"
      direction="rtl"
      :before-close="drawerClose"
      class="pageSetting"
    >
      <!-- 页面组件新增 -->
      <assembly
        v-if="rightDrawerType == 'assembly'"
        @addAssembly="addAssembly"
      ></assembly>
    </el-drawer>
  </div>
</template>
<script>
let _this
import middleware from '../../tuobiao/middleware/index'
import assembly from './assembly'
// import myMap from '../../components/maps/map'
export default {
  props: {
    settingConfig: {
      type: Object,
      // eslint-disable-next-line vue/require-valid-default-prop
      default: {}
    }
  },
  data() {
    return {
      rightDrawerType: '',
      settingDrawer: false, //右侧抽屉显示隐藏控制
      chooseType: ''
    }
  },
  components: {
    middleware,
    assembly,
    // myMap
  },
  mounted() {
    _this = this
    window.onresize = function() {
      _this.$refs['middleware'].resize()
      // _this.$refs['myMaps'].resize()
    }
  },
  methods: {
    //图表组件行点击事件
    rowClick(rowData) {
      if (rowData.marker) {
        this.$refs['myMaps'].addMarker(rowData.marker)
      }
    },
    //左侧菜单点击事件
    menuClick(menuItem) {
      this.$refs['middleware'].menuClick(menuItem)
      // this.$refs['myMaps'].menuClick(menuItem)
    },
    //内容区域宽高变化事件--菜单顶部宽度变化事件
    mainStyleChange() {
      this.$refs['middleware'].mainStyleChange()
      // this.$refs['myMaps'].resize()
    },
    //菜单数据传递
    // getMenuData(menuData) {
    //   this.$emit("getMenuData", menuData);
    // },
    //右侧抽屉显示事件
    rightDrawerShow(type) {
      this.rightDrawerType = type
      this.settingDrawer = true
    },
    //右侧抽屉关闭事件
    drawerClose() {
      this.settingDrawer = false
    },
    //页面组件新增事件
    addAssembly(type) {

      switch (type) {
        case 'tableChart': //图表组件集
          this.$refs['middleware'].addTemplate()
          break
        // case 'map': //行政区图
        //   this.$refs['myMaps'].addTemplate()
        //   break
      }
      this.settingDrawer = false
    }
  }
}
</script>
