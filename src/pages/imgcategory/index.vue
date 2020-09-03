<template>
  <view>
    <view class="category_tab">
      <view class="category_tab_title">
        <view class="category_inner">
          <uni-segmented-control
            :current="current"
            :values="items.map(v=>v.title)"
            @clickItem="onClickItem"
            style-type="text"
            active-color="#d4237a"
          ></uni-segmented-control>
        </view>
        <view class="iconfont iconsearch"></view>
      </view>
      <scroll-view @scrolltolower="handleScrolltoLower" enable-flex scroll-y class="category_content">
          <view class="vertical_item" v-for="(item,index) in vertical" :key="index">
              <image :src="item.thumb" mode="widthFix"></image>
          </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    uniSegmentedControl
  },
  data() {
    return {
      items: [{ title: "最新" ,order:"new"}, { title: "热门" ,order:"hot"}],
      current: 0,
      params:{
          limit:30,
          skip:0,
          order:"new"
      },
      id:0,
      vertical:[],
      hasMore:true
    };
  },
onLoad(options){
    this.id = options.id;
      this.getList();
},
  created() {},

  mounted() {
  },

  methods: {
    getList() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
        data:this.params
      }).then(res => {
        if(res.res.vertical.length === 0){
            this.hasMore = false;
            uni.showToast({
                title:"没有更多数据了",
                icon:"none"
            })
            return;
        }
        this.vertical = [...this.vertical, ...res.res.vertical];
      });
    },
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }else {
          return;
      }
      this.params.skip = 0;
      this.vertical = [];
      this.params.order = this.items[e.currentIndex].order;
      this.getList();
    },
    //加载下一页
    handleScrolltoLower(){
        if(this.hasMore) {
            this.params.skip += this.params.limit;
            this.getList();
        }else{
            uni.showToast({
                title:"没有更多数据了",
                icon:"none"
            })
        }
    }
  }
};
</script>
<style lang="scss" scoped>
.category_tab {
  .category_tab_title {
    position: relative;
    .category_inner {
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }
  .category_content {
      display: flex;
      flex-wrap: wrap;
      height: calc(100vh - 36px);
   .vertical_item {
       width: 33.33%;
       border: 3rpx solid #fff;
        image {

        }
    }
  }
}
</style>
