<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="tab border-bottom">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
        <!-- <a v-link="{path:'/goods'}">商品</a> -->
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
        <!-- <a v-link="{path:'/ratings'}">评价</a> -->
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
        <!-- <a v-link="{path:'/seller'}">商家</a> -->
      </div>
    </div>
    <keep-alive><router-view :seller='seller' keep-alive></router-view></keep-alive>
  </div>
</template>

<script>
// import {urlParse} from './common/js/util'
import hheader from './components/header/hheader' 
import axios from 'axios'
export default {
  name: 'App',
  data(){
    return {
      seller: {
        // id:(()=>{
        //   let queryParam = urlParse()
        //   return queryParam.id
        // })()
      }
    }
  },
  components: {
    'v-header':hheader,
  },
  methods: {
    getHomeInfo() {
      axios.get('/api/seller')
    	.then(this.getHomeInfoSucc)
  },
    getHomeInfoSucc(res) {
      res=res.data
      if (res.ret && res.data) {
         const data = res.data
          // this.city=data.city
    }
      this.seller=res.data
    }
  },
  mounted() {
    this.getHomeInfo()
//让页面挂载好后执行该函数
  }
}
</script>

<style lang="stylus" scoped>
  #app
    .tab
      display flex
      width 100%
      height 40px
      line-height 40px
      .tab-item
        flex 1
        text-align center
      .tab-item a
        display block
        font-size 14px
      .router-link-active
        color rgb(240,20,20)
</style>
