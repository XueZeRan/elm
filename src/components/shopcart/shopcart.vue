<template>
  <div>
    <div class="shopcart">
      <div class="content">
        <div class="content-left" @click="toggleList">
          <div class="logo-wrapper">
            <div class="logo" :class="{'highlight':totalCount>0}">
              <i class="icon-close" :class="{'highlight':totalCount>0}"></i>
            </div>
            <div class="num" v-show="totalCount>0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right" @click="pay">
          <div class="pay" :class="payClass">
            {{payDesc}}
          </div>
        </div>
      </div>
      <!-- <div class="ball-container">
        <transition name="drop">
          <div v-for="(ball,index) in balls" :key="index" v-show="ball.show" class="ball">
            <dir class="inner"></dir>
          </div>
        </transition>
      </div> -->
      <transition name='fold'>
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="content">
            <ul>
              <li class="food border-bottom" v-for="(food,index) in selectfoods" :key="index">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.price*food.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food='food'></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <transition name="fade"><div class="list-mask" @click="hideList" v-show="listShow"></div></transition>
  </div>
</template>

<script>
import cartcontrol from '../cartcontrol/cartcontrol'
import BScroll from 'better-scroll'
export default {
  name:'shopcart',
  data(){
    return {
      // balls:[
      //   {
      //     show:false
      //   },
      //   {
      //     show:false
      //   },
      //   {
      //     show:false
      //   },
      //   {
      //     show:false
      //   },
      //   {
      //     show:false
      //   },
      // ]
      fold:true,
      scroll11:null
    }
  },
  props:{
    selectfoods:{
      type:Array,
      default(){
        return []
      }
    },
    deliveryPrice:{
      type:Number,
      default:4
    },
    minPrice:{
      type:Number,
      default:20
    }
  },
  methods: {
    toggleList(){
      if(!this.totalCount){
        return
      }
      this.fold = !this.fold
    },
    empty(){
      this.selectfoods.forEach((food)=>{
        food.count=0
      })
    },
    hideList(){
      // console.log(111)
      return this.fold=true
    },
    pay(){
      if(this.totalPrice<this.minPrice){
        return
      }
      alert(`支付${this.totalPrice}元`)
    }
  },
  computed:{
    totalPrice(){
      let total = 0
      this.selectfoods.forEach((food)=>{
        total += food.price * food.count
      })
      return total
    },
    totalCount(){
      let count = 0
      this.selectfoods.forEach((food)=>{
        count+=food.count
      })
      return count
    },
    payDesc(){
      if(this.totalPrice === 0){
        return `￥${this.minPrice}元起送`
      }else if (this.totalPrice<this.minPrice){
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      }else{
        return `去结算`
      }
    },
    payClass(){
      if(this.totalPrice<this.minPrice){
        return 'not-enough'
      }else{
        return 'enough'
      }
    },
     listShow(){
       if(!this.totalCount){
         this.fold = true
         return false
       }
       let show = !this.fold
       return show
     }
  },
  components:{
    cartcontrol
  },
  mounted() {
    this.scroll11 = new BScroll(this.$refs.content,{click:true})
  },
}
</script>

<style lang='stylus' scoped>
  .shopcart
    position fixed
    left 0
    bottom 0
    z-index 50
    width 100%
    height 48px
    .content
      display flex
      background #141d27
      color rgba(255,255,255,0.4)
      // font-size 0
      .content-left
        flex 1
        .logo-wrapper
          display inline-block
          position relative
          box-sizing border-box
          vertical-align top
          border-radius 50%
          top -10px
          width 56px
          height 56px
          margin 0 12px
          padding 6px
          background #141d27
          .logo
            width 100%
            height 100%
            border-radius 50%
            text-align center
            background #2b343c
            &.highlight
              background rgb(0,160,220)
            .icon-close
              line-height 44px
              font-size 24px
              color #80858a
              &.highlight
                color #fff
          .num
            position absolute 
            top 0
            right 0
            width 24px
            height 16px
            line-height 16px
            text-align center
            border-radius 16px
            font-size 9px
            font-weight 700
            color #fff
            background rgb(240,20,20)
            //阴影效果
            box-shadow 0 4px 8px 0 rgba(0,0,0,0.4)
        .price
          display inline-block
          vertical-align top
          margin-top 12px
          line-height 24px
          padding-right 12px
          box-sizing border-box
          border-right 1px solid rgba(255,255,255,0.1)
          font-size 16px
          font-weight 700
          @media only screen and (max-width: 320px)
            margin 12px 0 0 0
            padding 0
          &.highlight
            color #fff
        .desc
          display inline-block
          vertical-align top
          margin 12px 0 0 12px
          line-height 24px
          font-size 10px
          @media only screen and (max-width: 320px)
            margin 12px 0 0 0
            padding 0 0 0 0
      .content-right
        flex 0 0 105px
        width 105px
        background #2b343c
        .pay
          height 48px
          line-height 48px
          text-align center
          font-size 12px
          font-weight 700
          &.not-enough
            background #2b333b
          &.enough
            background #00b43c
            color #fff
    .shopcart-list
      position absolute
      left 0
      bottom 48px
      z-index -1
      width 100%
      &.fold-enter-active, &.fold-leave-active 
        transition: all 0.4s linear    //过渡效果的 CSS 属性的名称、过渡效果需要多少时间、速度效果的速度曲线
      &.fold-enter, &.fold-leave-active 
        opacity: 0
        transform translate3d(0, 100%, 0) //这样可以开启硬件加速，动画更流畅，3D旋转，X轴位移24px
      .list-header
        height 40px
        line-height 40px
        padding 0 18px
        background #f3f5f7
        background 1px solid rgba(7,17,27,0.1)
        .title
          float left 
          font-size 14px
          color rgb(7,17,27)
        .empty
          float right 
          font-size 12px
          color rgb(0,160,220)
      .list-content
        padding 0 18px
        max-height 217px
        overflow hidden
        background #fff
        .food
          position relative
          padding 12px 0
          box-sizing border-box
          .name
            line-height 24px
            font-size 14px
            color rgb(7,17,27)
          .price
            position absolute
            right 90px
            bottom 12px
            line-height 24px
            font-size 14px
            font-weight 700
            color rgb(240,20,20)
          .cartcontrol-wrapper
            position absolute
            right 0
            bottom 6px
  .list-mask
    position fixed
    top 0
    left 0
    width 100%
    height 100%
    z-index 40
    background rgba(7,17,27,0.6)
    backdrop-filter blur(10px)
    &.fade-enter-active, &.fade-leave-active 
      transition: all 0.4s linear    //过渡效果的 CSS 属性的名称、过渡效果需要多少时间、速度效果的速度曲线s
    &.fade-enter, &.fade-leave-active 
      opacity: 0
      transform translate3d(0,24px,0) //这样可以开启硬件加速，动画更流畅，3D旋转，X轴位移24px
</style>