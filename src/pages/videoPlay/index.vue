<template>
  <view class="video_play">
      <image :src="videoObj.img"></image>
    <view class="video_tool">
      <view :class="['iconfont', muted?'iconjingyin':'iconshengyin']" @click="muted = !muted"></view>
      <view class="iconfont iconzhuanfa">
        <button open-type="share"></button>
      </view>
    </view>

    <view class="video_wrap">
      <video :muted="muted" :src="videoObj.video" objectFit="fill"></video>
    </view>

    <view class="download">
      <view class="download_btn" @click="handleDownLoad">下载高清</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
        videoObj:{},
        muted:false
    };
  },
  onLoad() {
    this.videoObj = getApp().globalData.video;
  },

  created() {},

  mounted() {},

  methods: {
    async handleDownLoad(){
      await uni.showLoading({
        title:"下载中"
      })
      const {tempFilePath} = (await uni.downloadFile({
        url:this.videoObj.video
      }))[1];
      await uni.saveVideoToPhotosAlbum({
        filePath:tempFilePath
      })
      uni.hideLoading();
      await uni.showToast({title:"下载成功"})
    }
  }
};
</script>
<style lang="scss" scoped>
.video_play {
    position: relative;
    image {
        position: absolute;
        width: 100vh;
        height: 100vh;
        z-index: -1;
        filter: blur(20px);
    }
  .video_tool {
      height: 80rpx;
      display: flex;
      justify-content: flex-end;
    .iconfont {
        width: 80rpx;
        color: #fff;
        font-size: 50rpx;
        border-radius: 40rpx;
        background-color: rgba(0,0,0,.2);
        display: flex;
        justify-content: center;
        align-items: center;
        margin-right: 20rpx;
    }
    .iconzhuanfa{
      position: relative;
      button {
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
      }
    }
  }

  .video_wrap {
      display: flex;
      justify-content: center;
    video {
        width: 360rpx;
        height: 600rpx;
    }
  }

  .download {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 30rpx;
    .download_btn {
        width: 360rpx;
        height: 80rpx;
        border-radius: 40rpx;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #fff;
        border: 2rpx solid #fff;
        background-color: rgba(0,0,0,.2);
    }
  }
}
</style>
