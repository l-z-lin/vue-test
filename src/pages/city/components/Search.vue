<template>
  <div>
    <div class="search">
      <input type="text" class="search-input" placeholder="输入城市名或拼音" 
             v-model="keyword" />
    </div>
    <div class="search-content border-bottom" ref="search" v-show="keyword">
      <ul>
        <li v-for="item of list" :key="item.id" class="search-item" @click="handleCityClick(item.name)">{{item.name}}</li>
        <li class="search-item border-bottom" v-show="hasNoData">没有找到匹配数据</li>
      </ul>
    </div>
  </div>
</template>

<script>
  import Bscroll from 'better-scroll'
  export default {
  	name: 'CitySearch',
    data () {
      return {
        keyword: '',
        list: [],
        timer: null
      }
    },
    props: {
      cities: Object
    },
    watch: {
      keyword () {
        if(this.timer) {
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
          if(!this.keyword){
              this.list = []
              return
            }
            const result = []
            for (let i in this.cities) {
              this.cities[i].forEach((value) => {
                 if( value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1){
                   result.push(value)
                 }
              })
            }
         this.list = result
        },100)
        
      }
    },
    mounted () {
      this.scroll = new Bscroll(this.$refs.search)
    },
    computed: {
      hasNoData () {
        return !this.list.length
      }
    },
    methods: {
      handleCityClick (city) {
        this.$store.commit('changeCity',city)
        this.$router.push('/')
      }
    },
    updated () {
      this.scroll.refresh()
    }
  } 
</script>

<style lang="stylus" scoped>
    @import '~styles/varibles.styl'
    .search
      height: .72rem
      padding: 0 .1rem
      background: $bgColor
      .search-input
        box-sizing: border-box
        width: 100%
        height: .62rem
        line-height: .62rem
        border-radius: .06rem
        color: #666
        padding: 0 .1rem
        text-align: center
    .search-content
       z-index: 1
       position: absolute
       top: 1.58rem
       bottom: 0
       left: 0
       right: 0
       overflow: hidden
       background: #666
       .search-item
         line-height: .62rem
         padding-left: .2rem
         background: #fff
         color: #666
</style>