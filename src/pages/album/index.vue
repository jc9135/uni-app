<template>
  <view>
      <view class="album_top">
          <view class="album_bg">
              <image :src="album.cover" mode="widthFix"></image>
              <view class="album_info">
                  <view class="album_name">{{album.name}}</view>
                  <view class="album_btn">关注专辑</view>
              </view>
          </view>
      </view>
      <view class="album_middle">
          <view class="album_middle_top">
              <image :src="album.user.avatar"></image>
              <view class="author_name">{{album.user.name}}</view>
          </view>
          <view class="album_middle_bottom">
              <text>{{album.desc}}</text>
          </view>
      </view>
      <view class="album_bottom">
          <view class="album_list" v-for="item in wallpaper" :key="item.id">
              <image mode="widthFix" :src="item.thumb+item.rule.replace('$<Height>',360)"></image>
          </view>
      </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      requestData: {
        limit: 30,
        order: "new",
        skip: 0,
        first:1
      },
      id:-1,
      album:{},
      wallpaper:[],
      hasMore:true
    };
  },
  onLoad(options) {
    this.id = options.id;
    console.log(options)
    this.getData();
  },
  onReachBottom(){
    if(this.hasMore) {
        this.requestData.skip += this.requestData.limit;
        this.getData()
    }else {
        uni.showToast({
            title:"没有更多数据了",
            icon:"none"
        })
    }
  },

  created() {},

  mounted() {},

  methods: {
    getData() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.requestData
      }).then(res => {
        this.requestData.first = 0;
        if(Object.keys(this.album).length === 0) {
            this.album = res.res.album;
        }
        if(res.res.wallpaper.length === 0) {
            this.hasMore = false;
            uni.showToast({
                title:"没有更多数据了",
                icon:"none"
            })
            return;
        }
        this.wallpaper = [...this.wallpaper, ...res.res.wallpaper];
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.album_top {
  .album_bg {
    position: relative;
    img {

    }

    .album_info {
        position: absolute;
        width: 100%;
        left: 0;
        bottom: 0;
        display: flex;
        justify-content: space-between;
        height: 80rpx;
        align-items: center;
        color: #fff;
        padding:  0 15rpx;
      .album_name {
          font-size: 40rpx;
      }

      .album_btn {
          background-color: $color;
          width: 152rpx;
          height: 60rpx;
          display: flex;
          justify-content: center;
          align-items: center;
          border-radius: 10rpx;
      }
    }
  }
}
.album_middle {
    padding: 20rpx;
    .album_middle_top{
        display: flex;
        padding: 10rpx 0;
        image {
            width: 50rpx;
            height: 50rpx;
        }
        .author_name {
            color: #000;
            margin-left: 15rpx;
        }
    }
    .album_middle_bottom{
        
    }
}
.album_bottom {
    display: flex;
    flex-wrap: wrap;
    .album_list {
        width: 33.33%;
        border: 3rpx solid #fff;
        image{
        }
    }
}
</style>
