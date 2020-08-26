<template>
  <view>
    <view>
      <uni-segmented-control
        :current="current"
        :values="items.map(v=>v.title)"
        @clickItem="onClickItem"
        style-type="text"
        active-color="#d4237a"
      ></uni-segmented-control>
      <view class="content">
        <view v-if="current === 0">
          <home-recommend></home-recommend>
        </view>
        <view v-if="current === 1">
          <home-categroy></home-categroy>
        </view>
        <view v-if="current === 2">
          <homenew></homenew>
        </view>
        <view v-if="current === 3">
          <home-album></home-album>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import homeAlbum from "./home-album";
import homeCategroy from "./home-category";
import homeRecommend from "./home-recommend";
import homenew from "./home-new";
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    homeAlbum,
    homeCategroy,
    homeRecommend,
    homenew,
    uniSegmentedControl
  },
  data() {
    return {
      items: [
        { title: "推荐" },
        { title: "分类" },
        { title: "最新" },
        { title: "专辑" }
      ],
      current: 0
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  }
};
</script>
<style lang="scss" scoped>
</style>
