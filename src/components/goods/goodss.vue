<template>
  <div class="goods">
    <div class="menu-wrapper" ref='aaa'>
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="menu-item">
          <span class="text border-bottom">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref='bbb'>
      <ul>
        <li v-for="(item,index) in goods" class="food-lish" :key="index">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food,indes) in item.foods" class="food-item border-bottom" :key="indes">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
export default {
  name:'goods',
  data(){
    return {
      goods: [],
      scroll1:null,
      scroll2:null,
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
  methods: {
    getHomeInfo() {
      axios.get('/api/goods')
    	.then(this.getHomeInfoSucc)
  },
    getHomeInfoSucc(res) {
      res=res.data
      if (res.ret && res.data) {
         const data = res.data
          // this.city=data.city
    }
      this.goods=res.data
      // console.log(this.goods)
    }
  },
  created(){
    this.classMap = ['decrease','discount','special','invoice','guarantee']
  },
  mounted() {
    this.getHomeInfo()
//让页面挂载好后执行该函数
    this.scroll1 = new BScroll(this.$refs.aaa,{})
    //this.$refs.aaa    ref=aaa
    this.scroll2 = new BScroll(this.$refs.bbb,{})
  },
}
</script>

<style lang='stylus' scoped>
  @import '../../common/stylus/mixin.styl'
  .goods
    display flex
    position absolute 
    top 174px
    bottom 46px
    width 100%
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background #eaeaea
      .menu-item
        display table
        height 54px
        width 56px
        line-height 14px
        padding 0 12px
        .icon
          display inline-block
          vertical-align top
          width 12px
          height 12px
          margin-right 4px
          background-size 12px 12px
          background-repeat no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display table-cell
          width 56px
          vertical-align middle
          font-size 12px
    .foods-wrapper
      flex 1
      .title
        padding-left 14px
        height 26px
        line-height 26px
        border-left 2px solid #eaeaea
        font-size 12px
        color rgb(147,153,159)
        background #eaeaea
      .food-item
        display flex
        margin 18px
        padding-bottom 18px
        .icon
          flex 0 0 57px
          margin-right 10px
        .content
          flex 1
          .name
            margin 2px 0 8px 0
            height 14px
            line-height 14px
            font-size 14px
            color rgb(7,17,27)
          .desc, .extra
            line-height 10px
            font-size 10px
            color rgb(147,153,159)
          .desc
            margin-bottom 8px
            line-height 12px
          .extra
            .count
             margin-right 12px
          .price
            font-weight 700
            line-height 24px
            .now
              margin-right 8px
              font-size 14px
              color rgb(240,20,20)
            .old
              text-decoration line-through
              font-size 10px
              color rgb(147,153,15)
            
</style>