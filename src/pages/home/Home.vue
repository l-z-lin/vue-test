<template>
   <div>
     <home-header></home-header>
     <home-swiper :swiperList="swiperList"></home-swiper>
     <home-icons :icons="iconList"></home-icons>
     <home-recommend :recommend="recommendList"></home-recommend>
     <home-weekend :weekend="weekendList"></home-weekend>
   </div>
</template>

<script>

import HomeHeader from './components/Header'
import HomeSwiper from './components/Swiper'
import HomeIcons from './components/icons'
import HomeRecommend from './components/Recommend'
import HomeWeekend from './components/Weekend'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data () {
    return {
      lastCity: '',
      swiperList: [],
      iconList: [],
      recommendList: [],
      weekendList: []
    }
  },
  methods: {
    getHomeInfo () {
      axios.get('/api/index.json?city=' + this.lastCity).then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc (res) {
      let resu = res.data
      if(resu.ret && resu.data){
         let data = resu.data
         this.swiperList = data.swiperList
         this.iconList = data.iconList
         this.recommendList = data.recommendList
         this.weekendList = data.weekendList
      }
    }
  },
  mounted: function() {
    this.getHomeInfo()
    this.lastCity = this.$store.state.city
  },
  activated () {
    if(this.lastCity !== this.$store.state.city) {
      this.lastCity = this.$store.state.city
      this.getHomeInfo()
    }
  }
}
</script>

<style>

</style>
