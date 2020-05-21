<template>
  <div class="home" style="height:100%">
    <div ref="container" id="container"></div>
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
        center: [116.397428, 39.90923],
        resizeEnable: true,
        zoom: 10
      });
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
</style>
