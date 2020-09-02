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
           <swiper-action @swiperAction="handleSwiperAction">
                <image mode="widthFix" :src="imgDetail.thumb"></image>
           </swiper-action>
       </view>
       <view class="user_rank">
           <view class="rank">
               <text class="iconfont icondianzan">{{imgDetail.rank}}</text>
           </view>
           <view class="collect">
               <text class="iconfont iconshoucang">收藏</text>
            </view>
       </view>
       <view class="album_wrap"  v-if="album.length">
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
       <view class="comment hot" v-if="hot.length">
           <view class="comment_title">
               <text class="iconfont iconhot1"></text>
               <text class="comment_text">最热评论</text>
           </view>
           <view class="comment_list">
               <view class="comment_item" v-for="(item,index) in hot" :key="index">
                   <view class="comment_user">
                       <view class="user_icon">
                           <image :src="item.user.avatar" mode="widthFix"></image>
                       </view>
                       <view class="user_name">
                           <view class="user_nickname">{{item.user.name}}</view>
                           <view class="user_time">{{item.cnTime}}</view>
                       </view>
                       <view class="user_badge">
                           <image v-for="(item2,index) in item.user.title" :key="index" :src="item2.icon"></image>
                       </view>
                   </view>
                   <view class="comment_desc">
                       <view class="comment_content">{{item.content}}</view>
                       <view class="comment_lick">
                           <text class="iconfont icondianzan">{{item.size}}</text>
                       </view>
                   </view>
               </view>
           </view>
       </view>
       <view class="comment new" v-if="comment.length">
           <view class="comment_title">
               <text class="iconfont iconpinglun"></text>
               <text class="comment_text">最新评论</text>
           </view>
           <view class="comment_list">
               <view class="comment_item" v-for="(item,index) in comment" :key="index">
                   <view class="comment_user">
                       <view class="user_icon">
                           <image :src="item.user.avatar" mode="widthFix"></image>
                       </view>
                       <view class="user_name">
                           <view class="user_nickname">{{item.user.name}}</view>
                           <view class="user_time">{{item.cnTime}}</view>
                       </view>
                       <view class="user_badge">
                           <image v-for="(item2,index) in item.user.title" :key="index" :src="item2.icon"></image>
                       </view>
                   </view>
                   <view class="comment_desc">
                       <view class="comment_content">{{item.content}}</view>
                       <view class="comment_lick">
                           <text class="iconfont icondianzan">{{item.size}}</text>
                       </view>
                   </view>
               </view>
           </view>
       </view>
   </view>
</template>

<script>
import moment from 'moment'
import swiperAction from '@/components/swiperAction'
moment.locale('zh-cn')
export default {
    components:{
        swiperAction
    },
    onLoad(){
        const {imgIndex} = getApp().globalData;
        this.imgIndex = imgIndex;
        this.getList();
    },

    data() {
        return {
            imgDetail:{},
            album:[],
            hot:[],
            comment:[],
            imgIndex:0
        }
    },

    created() {

    },


    mounted() {
        
    },

    methods: {
        getList(){
            const {imgList} = getApp().globalData;
            this.imgDetail = imgList[this.imgIndex];
            this.imgDetail.cnTime=moment(this.imgDetail.atime*1000).fromNow();
            this.getData(this.imgDetail.id);
        },
        getData(id){
            this.request({
                url:`http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
            })
            .then(res=>{
                console.log(res.res)
                this.album = res.res.album;
                res.res.hot.forEach(v=>v.cnTime=moment(v.atime*1000).fromNow());
                res.res.comment.forEach(v=>v.cnTime=moment(v.atime*1000).fromNow());
                this.hot = res.res.hot;
                this.comment = res.res.comment;
            })
        },
        handleSwiperAction(e){
            const {imgList} = getApp().globalData;
            if(e.direction == 'left'&&this.imgIndex<imgList.length - 1){
                this.imgIndex++;
                this.getList();
            }else if (e.direction == 'right'&&this.imgIndex>0) {
                this.imgIndex--;
                this.getList();
            }else {
                uni.showToast({
                    title:"没有数据了",
                    icon:"none"
                })
            }
        }
    }

}

</script>
<style lang="scss" scoped>
.high_img {
    border-bottom: 2rpx solid #ccc;
}
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
.comment {
    .comment_title {
        padding: 15rpx;
        .iconfont {
            color: red;
            font-size: 40rpx;
        }

        .comment_text {
            font-weight: 600;
            font-size: 28rpx;
            color: #666;
            margin-left: 10rpx;
        }
    }

    .comment_list {

    }
}
.comment_list {
    .comment_item {
        border-bottom: 3px solid #eee;
        .comment_user {
            display: flex;
            padding: 20rpx 0;
            .user_icon {
                width: 15%;
                display: flex;
                justify-content: center;
                align-items: center;
                image {
                    width: 90%;
                }
            }

            .user_name {
                flex: 1;
                .user_nickname {
                    color: #777;
                }

                .user_time {
                    font-size: 24rpx;
                    color: #ccc;
                    padding: 5rpx;
                }
            }

            .user_badge {
                image {
                    width: 40rpx;
                    height: 40rpx;
                    display: inline-block;
                }
            }
        }

        .comment_desc {
            display: flex;
            padding: 30rpx 0;
            .comment_content {
                flex: 1;
                padding-left: 15%;
                color: #000;
            }

            .comment_lick {
                text-align: right;
                .iconfont.icondianzan {

                }
            }
        }
    }
}
.new {
    .iconpinglun{
        color: aqua!important;
    }
}
</style>
