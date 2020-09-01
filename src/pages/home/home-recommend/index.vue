<template>
  <scroll-view @scrolltolower="handleToLower" class="recommends_view" scroll-y v-if="recommends.length>0">
    <!-- 推荐 -->
    <view class="recommends">
      <navigator :url="`/pages/album/index?id=${item.target}`" class="recommends_item" v-for="item in recommends" :key="item.id">
        <image mode="widthFix" :src="item.thumb"></image>
      </navigator>
    </view>
    <!-- 月份 -->
    <view class="month">
      <view class="month_top">
        <view class="month_top_info">
          <view class="month_info">
            <text> {{monthList.dd}} </text>
            / {{monthList.MM}}月
          </view>
          <view class="month_text"> {{monthList.title}} </view>
        </view>
        <view class="month_top_more">更多 > </view>
      </view>
      <view class="month_content">
        <view class="month_content_item" v-for="(item,index) in monthList.items" :key="item.id">
          <go-detail :list="monthList.items" :index="index">
            <image mode="aspectFill"  :src="item.img+item.rule.replace('$<Height>',360)"></image>
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 热门 -->
    <view class="hot">
      <view class="hot_title">
        <text>热门</text>
      </view>
      <view class="hot_content">
        <view class="hot_item" v-for="(item,index) in hotList" :key="item.id">
          <go-detail :list="hotList" :index="index">
            <image mode="widthFix" :src="item.thumb"></image>
          </go-detail>
        </view>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import { format, compareAsc } from 'date-fns'
import goDetail from '@/components/goDetail'
export default {
  components: {
    goDetail
  },
  data() {
    return {
      recommends:[],
      monthList:{},
      hotList:[],
      requestData:{
        limit:30,
        order:'hot',
        skip:0
      },
      hasMore:true
    };
  },
  mounted(){
    this.getData();
    uni.setNavigationBarTitle({
      title:"推荐"
    })
  },
  methods:{
    handleToLower(){
      if(this.hasMore){
        this.requestData.skip+=this.requestData.limit;
        this.getData();
      }else {
        uni.showToast({title:"没有数据了",icon:"none"})
      }
    },
    getData(){
      this.request({
        url:"http://157.122.54.189:9088/image/v3/homepage/vertical",
        data: this.requestData
      })
      .then(res=>{
        if(this.recommends.length === 0) {
          this.recommends = res.res.homepage[1].items;
          this.monthList = res.res.homepage[2];
          this.monthList.MM = format(new Date(this.monthList.stime), 'MM');
          this.monthList.dd = format(new Date(this.monthList.stime), 'dd');
        }
        if(res.res.vertical.length===0) {
          this.hasMore = false;
          uni.showToast({
              title:"没有更多数据了",
              icon:"none"
          })
          return;
        }
        this.hotList = [...this.hotList, ...res.res.vertical];
      })
    }
  }
}; 
</script>
<style lang="scss" scoped>
.recommends_view {
  height: calc(100vh - 36px);
}
.recommends {
  display: flex;
  flex-wrap: wrap;
  .recommends_item {
    width: 50%;
    border:4rpx solid #fff;
  }
}
.month {
  .month_top {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .month_top_info {
        display: flex;
      .month_info {
        color: $color;
        font-size: 30rpx;
        text {
          font-size: 34rpx;
          font-weight: 600;
        }
      }

      .month_text {
        font-size: 34rpx;
        color: #666;
        font-weight: 600;
        margin-left: 5px;
      }
    }

    .month_top_more {
      font-size: 24rpx;
      color: $color;
    }
  }

  .month_content {
    display: flex;
    flex-wrap: wrap;
    .month_content_item{
      width: 33.33%;
      border: 2px solid #fff;
    }
  }
}
.hot {
  .hot_title {
    padding: 20rpx;
    font-size: 26rpx;
    font-weight: 600;
    text {
      border-left:3px solid $color;
      padding-left: 15rpx;
    }
  }
  .hot_content {
    display: flex;
    flex-wrap: wrap;
    .hot_item {
      width: 33.33%;
      border: 5rpx solid #fff;
      image {

      }
    }
  }
}
</style>
