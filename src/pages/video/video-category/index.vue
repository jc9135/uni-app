<template>
   <view>5</view>
</template>

<script>
export default {
  props: {
    urlobj: Object
  },
  data() {
    return {
      list: []
    };
  },

  created() {},

  mounted() {
    this.getData();
  },
  watch: {
    urlobj() {
      this.getData();
    }
  },

  methods: {
    getData() {
      this.request({
        url: this.urlobj.url,
        data: this.urlobj.params
      }).then(res => {
        console.log(res.res);
        this.list = res.res.videowp;
      });
    }
  }
};

</script>
<style lang="scss" scoped>

</style>
