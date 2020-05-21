<template>
  <div class="home" style="height:100%">
    <div ref="container" id="container"></div>
    <div class="btn-group">
      <div @click="open">开始编辑</div>
      <div @click="close">结束编辑</div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import AMap from "AMap"; // 引入高德地图
export default {
  name: "Home",
  data() {
    return {
      map: null,
      polyEditor: null,
      clientHeight: ""
    };
  },
  watch: {
    // 如果 `clientHeight` 发生改变，这个函数就会运行
    clientHeight: function() {
      this.changeFixed(this.clientHeight);
    }
  },
  mounted() {
    this.clientHeight = `${document.documentElement.clientHeight}`;
    this.mapInit();
  },

  methods: {
    mapInit() {
      this.map = new AMap.Map("container", {
        resizeEnable: true,
        zoom: 14
      });
      console.log(this.map.getCenter().lat);
      let center = this.map.getCenter();
      // var path = [
      //   [114.403322, 38.020255],
      //   [114.410703, 38.797555],
      //   [114.402292, 38.792353],
      //   [114.389846, 38.791365]
      // ];
      const path = [
        [center.lng - 0.02, center.lat + 0.02],
        [center.lng + 0.02, center.lat + 0.02],
        [center.lng + 0.02, center.lat - 0.02],
        [center.lng - 0.02, center.lat - 0.02]
      ];
      console.log(path);
      var polygon = new AMap.Polygon({
        path: path,
        strokeColor: "#FF33FF",
        strokeWeight: 6,
        strokeOpacity: 0.2,
        fillOpacity: 0.4,
        fillColor: "#1791fc",
        zIndex: 50
      });

      this.map.add(polygon);
      // 缩放地图到合适的视野级别
      this.map.setFitView([polygon]);

      this.polyEditor = new AMap.PolyEditor(this.map, polygon);
      // this.polyEditor.open();
      // polyEditor.on("addnode", function(event) {
      //   log.info("触发事件：addnode");
      // });

      // polyEditor.on("adjust", function(event) {
      //   log.info("触发事件：adjust");
      // });

      // polyEditor.on("removenode", function(event) {
      //   log.info("触发事件：removenode");
      // });

      this.polyEditor.on("end", function(type, target) {
        console.log(type);
        console.log(target);
        // event.target 即为编辑后的多边形对象
      });
    },
    open() {
      this.polyEditor.open();
    },
    close() {
      this.polyEditor.close();
    },
    changeFixed(clientHeight) {
      //动态修改样式
      console.log(clientHeight);
      this.$refs.container.style.height = clientHeight + "px";
    }
  }
};
</script>
<style lang="scss" scoped>
#container {
  width: 100%;
  height: 100%;
}
.btn-group {
  position: fixed;
  bottom: 100px;
  right: 100px;
  background: red;
}
</style>
