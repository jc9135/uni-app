<template>
   <view  @touchstart="handleTouchStart" @touchend="handleTouchend">
       <slot></slot>
   </view>
</template>

<script>
export default {
  data() {
    return {
      startTime: 0,
      startX: 0,
      startY: 0
    };
  },

  created() {},

  mounted() {},

  methods: {
    // 按下事件
    handleTouchStart(event) {
      this.startTime = Date.now();
      this.startX = event.changedTouches[0].clientX;
      this.startY = event.changedTouches[0].clientY;
    },
    // 松开事件
    handleTouchend(event) {
      const endTime = Date.now();
      const endX = event.changedTouches[0].clientX;
      const endY = event.changedTouches[0].clientY;
      if (endTime - this.startTime > 2000) {
        return;
      }
      let direction = "";
      if (Math.abs(endX - this.startX) > 20&&Math.abs(endY - this.startY) < 20) {
        direction = endX - this.startX > 0 ? "right" : "left";
      }else {
          return;
      }
      this.$emit('swiperAction',{direction})
    }
  }
};

</script>
<style lang="scss" scoped>

</style>
