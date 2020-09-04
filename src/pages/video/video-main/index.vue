<template>
  <view>
    <scroll-view scroll-y enable-flex class="video_main" @scrolltolower="handelScrollToLower">
        <view class="video_item" v-for="(item,index) in list" :key="index" @click="handleGoVideo(item)">
            <image :src="item.img" mode="widthFix"></image>
        </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  props: {
    urlobj: Object
  },
  data() {
    return {
      list: [],
      hasMore:true
    };
  },

  created() {},

  mounted() {
    this.getData();
  },
  watch: {
    urlobj() {
      this.list = [];
      this.getData();
    }
  },

  methods: {
    getData() {
      this.request({
        url: this.urlobj.url,
        data: this.urlobj.params
      }).then(res => {
          if(res.res.videowp.length === 0){
              this.hasMore = false;
              uni.showToast({
                  title:"没有更多数据了",
                  icon:"none"
              })
              return;
          }
        this.list = [...this.list, ...res.res.videowp];
      });
    },
    handelScrollToLower(){
        if(this.hasMore){
            this.urlobj.params.skip+=this.urlobj.params.limit;
            this.getData()
        }else {
            uni.showToast({
                title:"没有更多数据了",
                icon:"none"
            })
        }
    },
    handleGoVideo(item){
        getApp().globalData.video = item;
        uni.navigateTo({
            url:"/pages/videoPlay/index"
        })
    }
  }
};
</script>
<style lang="scss" scoped>
.video_main {
    display: flex;
    flex-wrap: wrap;
    height: calc(100vh - 36px);
    .video_item{
        width: 33.33%;
        border: 3rpx solid #fff;
    }
}
</style>
