<template>
  <view class="album">
    <scroll-view class="album_scroll" scroll-y @scrolltolower="handleToLower">
      <view class="album_swiper">
        <swiper autoplay indicator-dots circular>
          <swiper-item v-for="item in bannerList" :key="item.id">
            <image :src="item.thumb"></image>
          </swiper-item>
        </swiper>
      </view>
      <view class="album_list">
        <navigator class="album_item" v-for="item in albumList" :key="item.id" :url="`/pages/album/index?id=${item.id}`">
          <view class="album_left">
            <image :src="item.cover" mode="aspectFill"></image>
          </view>
          <view class="album_right">
            <view class="album_name">{{item.name}}</view>
            <view class="album_desc">{{item.desc}}</view>
            <view class="album_btn">
              <view class="album_attention"> + 关注</view>
            </view>
          </view>
        </navigator>
      </view>
    </scroll-view>
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
      requestData:{
        limit:30,
        order:'hot',
        skip:0
      },
      hasMore:true
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
        if(this.bannerList.length === 0) {
          this.bannerList = res.res.banner;
        }
        if(res.res.album.length === 0 ){
          this.hasMore = false;
          uni.showToast({
              title:"没有更多数据了",
              icon:"none"
          })
          return;
        }
        this.albumList = [...this.albumList, ...res.res.album];
      });
    },
    handleToLower(){
      if(this.hasMore) {
        this.requestData.skip += this.requestData.limit;
        this.getData();
      }else {
        uni.showToast({
          title:"没有数据了",
          icon:"none"
        })
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.album_scroll {
  height: calc(100vh - 36px);
}
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
