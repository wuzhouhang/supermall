<template>
  <div id="home">
    <NavBar class="home-nav"><div slot="center">购物车</div></NavBar>
    <HomeSwiper :banners="this.banners"></HomeSwiper>
    <HomeRecommendView :recommends="this.recommends"></HomeRecommendView>
    <FeatureView></FeatureView>
    <TabControl
      @tabClick="tabClick"
      class="tab-control"
      :title="['流行', '新款', '精选']"
    ></TabControl>
    <GoodsList :goods="goods[currentType].list"></GoodsList>
  </div>
</template>

<script>
import HomeSwiper from "./childComponents/HomeSwiper.vue";
import HomeRecommendView from "./childComponents/HomeRecommendView.vue";
import FeatureView from "./childComponents/FeatureView.vue";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl.vue";
import GoodsList from "components/content/goods/GoodsList.vue";

import { getHomeMultidata, getHomeGoods } from "network/home";

export default {
  name: "Home",
  props: {},
  components: {
    HomeSwiper,
    HomeRecommendView,
    FeatureView,

    NavBar,
    TabControl,
    GoodsList,
  },

  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
    };
  },

  created() {
    //请求多个数据
    this.getHomeMultidata();
    //请求商品数据
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },

  methods: {
    /**
     *事件监听方法
     *
     */
    tabClick(index) {
      if (index == 0) {
        this.currentType = "pop";
      } else if (index == 1) {
        this.currentType = "new";
      } else if (index == 2) {
        this.currentType = "sell";
      }
    },

    /**
     * 网络请求相关方法
     *
     */
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
      });
    },
  },
};
</script>

<style type="text/css" scoped>
#home {
  padding-top: 44px;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 99;
}
.tab-control {
  position: sticky;
  top: 44px;
  z-index: 99;
}
</style>
