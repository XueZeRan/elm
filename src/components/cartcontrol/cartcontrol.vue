<template>
  <div class="cartcontrol">   
    <!-- //减号和数字平移动画 -->
    <transition name="fade">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseClick">
        <span class="inner icon-shopping_cart"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-thumb_down" @click.stop.prevent="addClick"></div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  name:'cartcontrol',
  props:{
    food:{
      type:Object
    }
  },
  methods: {
    addClick(){
      // console.log(111)
      if(!this.food.count){
        Vue.set(this.food,'count',1)
        // this.food.count=1
      }else{
        this.food.count++
      }
    },
    decreaseClick(){
      // console.log(222)
      if(this.food.count>0){
        this.food.count--
      }
    }
  },
  created(){
    // console.log(this.food)
  }
}
</script>

<style lang='stylus' scoped>
  .cartcontrol
    font-size 0
    .cart-decrease
      display inline-block
      padding 6px
      &.fade-enter-active, &.fade-leave-active 
        transition: all 0.4s linear    //过渡效果的 CSS 属性的名称、过渡效果需要多少时间、速度效果的速度曲线
        transform rotate(-180)
      &.fade-enter, &.fade-leave-active 
        opacity: 0
        transform translate3d(24px, 0, 0) //这样可以开启硬件加速，动画更流畅，3D旋转，X轴位移24px
        transform rotate(180deg)
      .inner
        display inline-block //设置成inline-block才有高度，才能有动画
        font-size 24px
        line-height 24px
        // vertical-align top
        color rgb(0,160,220)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      line-height 24px
      text-align center
      font-size 10px
      color rgb(147,153,159)
    .cart-add
      display inline-block
      padding 6px
      font-size 24px
      line-height 24px
      color rgb(0,160,220)
</style>