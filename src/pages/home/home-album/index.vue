<template>
  <view class="album">
      <view class="album_swiper">
        <swiper autoplay indicator-dots circular>
          <swiper-item v-for="item in bannerList" :key="item.id">
            <image :src="item.thumb"></image>
          </swiper-item>
        </swiper>
      </view>
      <view class="album_list">
        <view class="album_item" v-for="item in albumList" :key="item.id">
          <view class="album_left">
            <image :src="item.cover"></image>
          </view>
          <view class="album_right">
            <view class="album_name">{{item.name}}</view>
            <view class="album_desc">{{item.desc}}</view>
            <view class="album_btn">
              <view class="album_attention"> + 关注</view>
            </view>
          </view>
        </view>
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
.album_list {
  padding:10rpx;
  .album_item {
    padding:10rpx 0;
    display: flex;
    border-bottom: 1rpx solid #eee;
    .album_left {
      flex: 1;
      padding: 10rpx;
      image {
        height: 200rpx;
        width: 200rpx;
      }
    }
    .album_right {
      flex: 2;
      padding: 0 10rpx;
      overflow: hidden;
      .album_name {
        font-size: 30rpx;
        color: #000;
        padding: 10rpx 0;
      }
      .album_desc {
        font-size: 24rpx;
        padding: 10rpx 0;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }
      .album_btn {
        padding: 10rpx;
        display: flex;
        justify-content: flex-end;
        .album_attention {
          color: $color;
          border: 1rpx solid $color;
          padding: 10rpx;
        }
      }
    }
  }
}
</style>
