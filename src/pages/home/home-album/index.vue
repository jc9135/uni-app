<template>
  <view class="album">
      <view class="album_swiper">
        <swiper autoplay indicator-dots circular>
          <swiper-item v-for="item in bannerList" :key="item.id">
            <image :src="item.thumb"></image>
          </swiper-item>
        </swiper>
      </view>
  </view>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      requestData: {
        limit: 30,
        order: "album",
        skip: 0
      },
      albumList:[],
      bannerList:[],
    };
  },
  mounted() {
    // 设置页面的标题
    uni.setNavigationBarTitle({
      title: "专辑"
    });
    this.getData();
  },
  methods: {
    getData() {
      this.request({
        url: "http://157.122.54.189:9088/image/v1/wallpaper/album",
        data: this.requestData
      }).then(res => {
        this.bannerList = res.res.banner;
        this.albumList = res.res.album;
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.album_swiper {
  swiper {
    height: calc(750rpx/2.3);
    image {
      height: 100%;
    }
  }
}
</style>
