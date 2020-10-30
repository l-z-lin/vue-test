<template>
 <div class="wrapper">
  <swiper ref="mySwiper" :options="swiperOptions" v-if="showSwiper">
    <swiper-slide v-for="item of swiperList" :key="item.id">
      <img class="swiper-img" :src="item.imgUrl" />
    </swiper-slide>
    <div class="swiper-pagination" slot="pagination"></div>
  </swiper>
 </div>
</template>

<script>
  export default {
  name: 'HomeSwiper',
  data () {
    return {
      swiperOptions: {
        pagination: '.swiper-pagination',
        loop: true
      }
    }
  },
  props: {
    swiperList: Array
  },
  computed: {
    showSwiper () {
      return this.swiperList.length
    }
  }
}
</script>

<style lang="stylus" scoped>
  .wrapper >>> .swiper-pagination-bullet-active
    background: #fff
  .wrapper
    overflow: hidden
    width: 100%
    height: 0
    padding-bottom: 31.25%
    background: #eee
    position: relative
    .swiper-container
      position: static
      .swiper-img
        width: 100%
</style>