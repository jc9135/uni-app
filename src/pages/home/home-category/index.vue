<template>
  <view class="category">
    <navigator class="category_item" v-for="(item,index) in categoryList" :key="index" :url="`/pages/imgcategory/index?id=${item.id}`">
      <image :src="item.cover" mode="aspectFill"></image>
      <view class="category_name">{{item.name}}</view>
    </navigator>
  </view>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      categoryList:[]
    };
  },
  mounted() {
    uni.setNavigationBarTitle({
      title: "分类"
    });
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url:"http://157.122.54.189:9088/image/v1/vertical/category"
      })
      .then(res=>{
        console.log(res)
        this.categoryList = res.res.category;
      })
    }
  }
};
</script>
<style lang="scss" scoped>
.category {
  display: flex;
  flex-wrap: wrap;
  .category_item{
    width: 33.3%;
    border:  3rpx solid #fff;
    position: relative;
    image {
      height: 240rpx;
    }
    .category_name {
      position: absolute;
      bottom: 0;
      left: 0;
      color: #fff;
      font-size: 34rpx;
      width: 100%;
      height: 50rpx;
      background-image: linear-gradient(to right top,rgba(0, 0, 0, 0.2),rgba(0, 0, 0, 0));
      display: flex;
      align-items: center;
      padding-left: 20rpx;
    }
  }
}
</style>
