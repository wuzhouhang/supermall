<template>
  <Swiper>
    <SwiperItem :key="index" v-for="(item, index) in banners">
      <a :href="item.link">
        <img :src="item.image" alt="" />
      </a>
    </SwiperItem>
  </Swiper>
</template>

<script>
import { Swiper, SwiperItem } from "components/common/swiper";
export default {
  name: "HomeSwiper",
  props: {
    banners: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  components: {
    Swiper,
    SwiperItem,
  },
};
</script>

<style type="text/css" scoped>
</style>
