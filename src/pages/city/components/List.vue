<template>
	<div class="list" ref="wrapper">
    <div>
       <div class="area">
         <div class="title border-topbottom">当前城市</div>
         <div class="button-list">
            <div class="button-wrapper">
              <div class="button">{{this.currentCity}}</div>
            </div>
         </div>
       </div>
       <div class="area">
         <div class="title border-topbottom">热门城市</div>
         <div class="button-list">
            <div class="button-wrapper" v-for="item of hotCities" :key="item.id" @click="handleCityClick(item.name)">
              <div class="button">{{item.name}}</div>
            </div>
         </div>
       </div>
       <div class="area" v-for="(item,key) in cities" :key="key" :ref="key">
         <div class="title border-topbottom">{{key}}</div>
         <div class="item-list">
            <div class="item border-bottom" v-for="innerItem of item" :key="innerItem.id" @click="handleCityClick(innerItem.name)">{{innerItem.name}}</div>
         </div>        
       </div>
    </div>  
  </div>
</template>

<script>
  import Bscroll from 'better-scroll'
  import { mapState } from 'vuex'
  export default {
  	name: 'CityList',
    props: {
      cities: Object,
      hotCities: Array,
      letter: String
    },
    computed: {
      ...mapState({
        currentCity: 'city'
      })
    },
    mounted () {
      this.scroll = new Bscroll(this.$refs.wrapper,{
      click: true
    })
    },
    updated () {
      this.scroll.refresh()
    },
    methods: {
      handleCityClick (city) {
        this.$store.commit('changeCity',city)
        this.$router.push('/')
      }
    },
    watch: {
      letter(){
        if (this.letter) {
          const element = this.$refs[this.letter][0]
          this.scroll.scrollToElement(element)
        }
      }
    },
    activated () {
      this.scroll.refresh()
    }
  } 
</script>

<style lang="stylus" scoped>
    @import '~styles/varibles.styl'
    .border-topbottom
      &:before
        border-color: #ccc
      &:after
        border-color: #ccc
    .border-bottom
      &:before
        border-color: #ccc
    .list
      position: absolute
      top: 1.58rem
      left: 0
      right: 0
      bottom :0
      overflow: hidden
      .title
        height: .54rem
        line-height: .54rem
        color: #666
        padding-left: .2rem
        background: #eee
        font-size: .26rem
      .button-list
        overflow :hidden
        .button-wrapper
          width: 33.33%
          float: left
          .button
            border: .02rem solid #ccc
            border-radius: .06rem
            text-align:center
            padding: .1rem 0
            margin: .1rem
      .item-list
        .item
         line-height: .76rem
         padding-left: .2rem
</style>