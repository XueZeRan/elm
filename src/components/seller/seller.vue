<template>
  <div class="seller" ref="sellerBS">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-bottom">
          <s-tarr :size='36' :score='seller.score'></s-tarr>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>分钟
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-bottom">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul class="supports" v-if="seller.supports">
          <li class="support-item border-bottom" v-for='(item,index) in seller.supports' :key="index">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="personWrap">
          <ul class="pic-list" ref='personTab'>
            <li class="pic-item" v-for="(pic,picindex) in seller.pics" :key="picindex">
              <img :src="pic" width="120" height="90" alt="">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title border-bottom">商家信息</h1>
        <ul>
          <li class="info-item border-bottom" v-for="(info,index) in seller.infos" :key='index'>{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import STarr from '../star/STarr'
import split from '../split/split'
export default {
  name:'seller',
  data() {
    return {
      scroll11:null,
      favorite:false
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
  components:{
    STarr,
    split
  },
  computed:{
    favoriteText(){
      return this.favorite? '已收藏' : "收藏"
    }
  },
  methods: {
    toggleFavorite(){
      this.favorite=!this.favorite
    },
    personScroll() {
      // 默认有六个li子元素，每个子元素的宽度为120px
      let width = 4 * 125;
      this.$refs.personTab.style.width = width + "px";
      // this.$nextTick 是一个异步函数，为了确保 DOM 已经渲染
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.personWrap, {
            startX: 0,
            click: true,
            scrollX: true,
            // 忽略竖直方向的滚动
            scrollY: false,
            eventPassthrough: "vertical"
          });
        } else {
          this.scroll.refresh();
        }
      });
    }
  
  },
  created(){
    this.classMap = ['decrease','discount','special','invoice','guarantee'];
    this.$nextTick(()=>{
    this.personScroll();
    });
  },
  mounted() {
    this.scroll11 = new BScroll(this.$refs.sellerBS,{click:true})
  }
}
</script>

<style lang='stylus'scoped>
@import '../../common/stylus/mixin.styl'
  .seller
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      position relative
      padding 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .desc
        padding-bottom 18px
        font-size 0
        .star
          display inline-block
          margin-right 8px
          vertical-align top
        .text
          display inline-block
          margin-right 12px
          line-height 18px
          vertical-align top
          font-size 10px
          color rgb(77,85,93)
      .remark
        display flex
        padding-top 18px
        .block
          flex 1
          text-align center
          border-right 1px solid rgba(7.17.27.0.1)
          &:last-child
            border none
          h2
            margin-bottom 4px
            line-height 10px
            font-size 10px
            color rgb(147,153,159)
          .content
            line-height 24px
            font-size 10px
            color rgb(7,17,27)
            .stress
              font-size 24px
      .favorite
        position absolute
        right 11px
        top 18px
        width 50px
        text-align center
        .icon-favorite
          display block
          margin-bottom 4px
          line-height 24px
          font-size 24px
          color #d4d6d9
          &.active
            color rgb(240,20,20)
        .text
          line-height 10px
          font-size 10px
          color rgb(77,85,93)
    .bulletin
      padding 18px 18px 0 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .content-wrapper
        padding 0 12px 16px 12px
        .content
          line-height 24px
          font-size 12px
          color rgb(240,20,20)
      .supports
        .support-item
          padding 16px 12px
          font-size 0
          // &:last-child
          //   border-none()
          .icon
            display inline-block
            width 16px
            height 16px
            vertical-align top
            margin-right 6px
            background-size 16px 16px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_4')
            &.discount
              bg-image('discount_4')
            &.guarantee
              bg-image('guarantee_4')
            &.invoice
              bg-image('invoice_4')
            &.special
              bg-image('special_4')
          .text
            line-height 16px
            font-size 12px
            color rgb(7,17,27)
    .pics
      padding 18px
      .title
        margin-bottom 12px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .pic-wrapper
        width 100%
        overflow hidden
        white-space nowrap //超出宽度不被换行
        .pic-list
          font-size 0
          .pic-item
            display inline-block
            margin-right 6px
            width 120px
            height 90px
            &:last-child
              margin 0
    .info
      padding 18px 18px 0 18px
      color rgb(7,17,27)
      .title
        padding-bottom 12px
        line-height 14px
        font-size 14px
      .info-item
        padding 16px 12px
        line-height 16px
        font-size 12px

      
</style>