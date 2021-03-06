<template>
  <div>
    <transition name="move">
      <div v-show="showFlag" ref="move" class="food">
        <div class="food-content">
          <div class="image-header">
            <img :src="food.image" alt="">
            <div class="back" @click="hide">
              <i class="icon-add_circle"></i>
            </div>
          </div>
          <div class="content">
            <h1 class="title">{{food.name}}</h1>
            <div class="detail">
              <span class="sell-count">月售{{food.sellCount}}份</span>
              <span class="rating">好评率{{food.rating}}%</span>
            </div>
            <div class="price">
              <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
            </div>
            <div class="cartcountrol-wrapper">
            <cartcountrol :food='food'></cartcountrol>
            </div>
            <div @click.stop.prevent="addFirst(food)" class="buy" v-show="!food.count||food.count===0">加入购物车</div>
          </div>
          <split v-show="food.info"></split>
          <div class="info" v-show="food.info">
            <h1 class="title">商品信息</h1>
            <p class="text">{{food.info}}</p>
          </div>
          <split></split>
          <div class="rating">
            <h1 class="title">商品评价</h1>
            <ratingselect @ratingtype='ratingtypedata' @ratingcontent='ratingcontentdata' :selectType='selectType' :onlyContent='onlyContent' :desc='desc' :ratings='food.ratings'></ratingselect>
            <div class="rating-wrapper">
              <ul v-show="food.ratings && food.ratings.length">
                <li v-show="needShow(rating.rateType,rating.text)" v-for="(rating,index) in food.ratings" :key="index" class="rating-item border-bottom">
                  <div class="user">
                    <span class="name">{{rating.username}}</span>
                    <img class="avatar" width="12" height="12" :src="rating.avatar" alt="">
                  </div>
                  <div class="time">{{rating.rateTime | formatDate}}</div>
                  <p class="text">
                    <span :class="{'icon-arrow_lift':rating.rateType===0,'icon-check_circle':rating.rateType===1}"></span>{{rating.text}}
                  </p>
                </li>
              </ul>
              <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
            </div>
          </div>
          
        </div>
      </div>
    </transition>
  </div>
  
</template>

<script>
import BScroll from 'better-scroll'
import Vue from 'vue'
import {formatDate} from '../../common/js/data'
import cartcountrol from '../cartcontrol/cartcontrol'
import ratingselect from '../ratingselect/ratingselect'
import split from '../split/split'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  name:'food',
  props:{
    food:{
      type:Object
    }
  },
  data() {
    return {
      showFlag:false,
      scroll11:null,
      selectType: ALL,
      onlyContent:false,
      desc:{
        all:'全部',
        positive:'推荐',
        negative:'吐槽'
      }
    }
  },
  methods: {
    show(){
      this.showFlag = true
      // this.selectType= All
      // this.onlyContent=true
    },
    hide(){
      this.showFlag = false
    },
    addFirst(food){
    // 引入vue的方法获取数据来改变数据
      Vue.set(this.food,'count',1)
    },
    needShow(type,text){
      if(this.onlyContent && !text){
        return false
      }
      if(this.selectType === ALL){
        return true
      }else{
        return type===this.selectType
      }
    },
    ratingtypedata(data){
      this.selectType=data
    },
    ratingcontentdata(data){
      this.onlyContent=data
    }
  },
  filters:{
    formatDate(time){
      let date = new Date(time)
      return formatDate(date,'yyyy-MM-dd hh:mm')
    }
  },
  components:{
    cartcountrol,
    split,
    ratingselect
  },
   mounted() {
    this.scroll11 = new BScroll(this.$refs.move,{click:true})
  }
}
</script>

<style lang='stylus' scoped>
  .food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #ffffff
    &.move-enter-active, &.move-leave-active 
      transition: all 0.2s linear    //过渡效果的 CSS 属性的名称、过渡效果需要多少时间、速度效果的速度曲线
    &.move-enter, &.move-leave-active 
      opacity: 0
      transform translate3d(100%, 0, 0) //这样可以开启硬件加速，动画更流畅，3D旋转，X轴位移24px
    .image-header
      position relative
      width 100%
      height 0
      padding-top 100%
      img 
        position absolute
        top 0
        left 0
        width 100%
        height 100%
      .back
        position absolute
        top 10px
        left 0
        border-radius 50%
        background rgba(0,0,0,0.1)
        .icon-add_circle
          display block
          padding 10px
          font-size 20px
          color #fff
    .content
      padding 18px
      position relative
      .title
        line-height 14px
        margin-bottom 8px
        font-size 14px
        font-weight 700
        color rgb(7,17,27)
      .detail
        margin-bottom 18px
        line-height 10px
        height 10px
        font-size 0
        .sell-count,.rating
          font-size 10px
          color rgb(147,153,159)
        .sell-count
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
      .cartcountrol-wrapper
        position absolute
        right 12px
        bottom 12px
      .buy
        position absolute
        right 18px
        bottom 18px
        z-index 10px
        height 24px
        line-height 24px
        padding 0 12px
        box-sizing border-box
        border-radius 12px
        font-size 10px
        color #fff
        background rgb(0,160,220)
    .info
      padding 18px
      .title
        line-height 14px
        margin-bottom 6px
        font-size 14px
        color rgb(7,17,27)
      .text
        line-height 24px
        padding 0 8px
        font-size 12px
        color rgb(77,85,93)
    .rating
      padding-top 18px
      .title
        line-height 14px
        margin-left 18px
        font-size 14px
        color rgb(7,17,27)
      .rating-wrapper
        padding 0 18px
        .rating-item
          position relative
          padding 16px 0
          .user
            position absolute
            right 0
            top 16px
            line-height 12px
            font-size 0
            .name
              display inline-block
              margin-right 6px
              vertical-align top
              font-size 10px
              color rgb(147,153,159)
            .avatar
              border-radius 50%
          .time
            margin-bottom 6px
            line-height 12px
            font-size 10px
            color rgb(147,153,159)
          .text
            line-height 16px
            font-size 12px
            color rgb(7,17,27)
            .icon-arrow_lift,.icon-check_circle
              margin-right 4px
              line-height 16px
              font-size 12px
            .icon-arrow_lift
              color rgb(0,160,220)
            .icon-check_circle
              color rgb(147,153,159)
        .no-rating
          padding 16px 0
          font-size 12px
          color rgb(147,153,159)
</style>