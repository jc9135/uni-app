<template>
   <view>
       <view class="user_info">
           <view class="user_icon">
               <image :src="imgDetail.user.avatar" mode="widthFix"></image>
           </view>
           <view class="user_desc">
               <view class="user_name">{{imgDetail.user.name}}</view>
               <view class="user_time">{{imgDetail.cnTime}}</view>
           </view>
       </view>
       <view class="high_img">
           <image mode="widthFix" :src="imgDetail.newThumb"></image>
       </view>
       <view class="user_rank">
           <view class="rank">
               <text class="iconfont icondianzan">{{imgDetail.rank}}</text>
           </view>
           <view class="collect">
               <text class="iconfont iconshoucang">收藏</text>
            </view>
       </view>
       <view class="album_wrap">
           <view class="album_title">相关</view>
           <view class="album_list">
                <view class="album_item" v-for="item in album" :key="item.id">
                    <view class="album_cover">
                        <image :src="item.cover" mode="aspectFill"></image>
                    </view>
                    <view class="album_info">
                        <view class="album_info_text">专辑</view>
                        <view class="album_name">{{item.name}}</view>
                        <text class="iconfont iconiconfontjiantou4"></text>
                    </view>
                </view>
           </view>
       </view>
   </view>
</template>

<script>
import moment from 'moment'
moment.locale('zh-cn')
export default {
    onLoad(){
        console.log(getApp().globalData)
        const {imgList,imgIndex} = getApp().globalData;
        this.imgDetail = imgList[imgIndex];
        this.imgDetail.newThumb=this.imgDetail.thumb+this.imgDetail.rule.replace('$<Height>',360);
        this.imgDetail.cnTime=moment(this.imgDetail.atime*1000).fromNow();
        this.getData(this.imgDetail.id);
    },

    data() {
        return {
            imgDetail:{},
            album:[],
            hot:[],
            comment:[]
        }
    },

    created() {

    },


    mounted() {
        
    },

    methods: {
        
        getData(id){
        this.request({
            url:`http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
        })
        .then(res=>{
            console.log(res.res)
            this.album = res.res.album;
            this.hot = res.res.hot;
            this.comment = res.res.comment;
        })
        }
    }

}

</script>
<style lang="scss" scoped>
.user_info {
    display: flex;
    padding: 20rpx;
  .user_icon {
      padding: 0 20rpx;
    image {
        width: 88rpx;
        border-radius: 50%;
    }
  }

  .user_desc {
    .user_name {
        color: #000;
        font-weight: 600;
    }

    .user_time {
        color: #ccc;
        font-size: 24rpx;
        padding: 10rpx 0;
    }
  }
}
.user_rank{
    display: flex;
    height: 80rpx;
    border-bottom: 3rpx solid #eee;
    .rank {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        .icondianzan {

        }
    }

    .collect {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        .iconshoucang {

        }
    }
}
.album_wrap{
    padding: 20rpx;
    .album_title {
        padding: 10rpx 0;
    }

    .album_list {
        .album_item {
            display: flex;
            padding: 10rpx 0;
            border-bottom: 3rpx solid #ccc;
            .album_cover {
                flex: 1;
                image {
                    width: 180rpx;
                    height: 180rpx;
                }
            }

            .album_info {
                flex: 3;
                padding-left: 20rpx;
                position: relative;
                .iconfont {
                    position: absolute;
                    font-size: 40rpx;
                    top: 50%;
                    transform: translateY(-50%);
                    right: 10%;
                    color: #000;
                }
                .album_info_text {
                    width: 100rpx;
                    height: 50rpx;
                    background-color: $color;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                }

                .album_name {
                    padding: 10rpx 0;
                    color: #888;
                }
            }
        }
    }

}
</style>
