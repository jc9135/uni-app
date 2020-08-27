<template>
  <view>
    <!-- 推荐 -->
    <view class="recommends">
      <view class="recommends_item" v-for="item in recommends" :key="item.id">
        <image mode="widthFix" :src="item.thumb"></image>
      </view>
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
        <view class="month_content_item" v-for="item in monthList.items" :key="item.id">
          <image mode="aspectFill"  :src="item.img+item.rule.replace('$<Height>',360)"></image>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { format, compareAsc } from 'date-fns'
export default {
  components: {},
  data() {
    return {
      recommends:[],
      monthList:{},
      hotList:[]
    };
  },
  mounted(){
    this.request({
      url:"http://157.122.54.189:9088/image/v3/homepage/vertical",
      data:{
        limit:30,
        order:'hot',
        skip:0
      }
    })
    .then(res=>{
      this.recommends = res.res.homepage[1].items;
      this.monthList = res.res.homepage[2];
      this.hotList = res.res.vertical;
      this.monthList.MM = format(new Date(this.monthList.stime), 'MM');
      this.monthList.dd = format(new Date(this.monthList.stime), 'dd');
    })
  }
}; 
</script>
<style lang="scss" scoped>
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
</style>
