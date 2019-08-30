<template>
  <div class="ratings" ref="ratingBs">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <s-tarr :size='36' :score='seller.serviceScore'></s-tarr>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <s-tarr :size='36' :score='seller.foodScore'></s-tarr>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect @ratingtype='ratingtypedata' @ratingcontent='ratingcontentdata' :selectType='selectType' :onlyContent='onlyContent' :desc='desc' :ratings='ratings'></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="(rating,index) in ratings" :key="index" v-show="needShow(rating.rateType,rating.text)" class="rating-item border-bottom ">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar" alt="">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <s-tarr :size='24' :score='rating.score'></s-tarr>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliverTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-arrow_lift"></span>
                <span class="item border" v-for="(item,indextwo) in rating.recommend" :key="indextwo">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import STarr from '../star/STarr'
import ratingselect from '../ratingselect/ratingselect'
import split from '../split/split'
import {formatDate} from '../../common/js/data'

const ALL = 2

export default {
  name:'ratings',
  props:{
    seller:{
      type:Object
    }
  },
  data() {
    return {
      ratings:[],
      showFlag:false,
      selectType: ALL,
      onlyContent:false,
      desc:{
        all:'全部',
        positive:'推荐',
        negative:'吐槽'
      },
      scroll11:null
    }
  },
  components:{
    STarr,
    ratingselect,
    split
  },
  methods: {
    getHomeInfo() {
      axios.get('/api/ratings')
    	.then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc(res) {
      res=res.data
      if (res.ret && res.data) {
         const data = res.data
          // this.city=data.city     
      }
      this.ratings=res.data
      // console.log(this.goods)
    //   this.$nextTick(()=>{  // DOM更新之后获取子元素
    //     this._calculateHeight()
    // })  
    },
    ratingtypedata(data){
      this.selectType=data
    },
    ratingcontentdata(data){
      this.onlyContent=data
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
    }
  },
  filters:{
    formatDate(time){
      let date = new Date(time)
      return formatDate(date,'yyyy-MM-dd hh:mm')
    }
  },
  mounted(){
    this.getHomeInfo(),
    this.scroll11 = new BScroll(this.$refs.ratingBs,{click:true})
  }
}
</script>

<style lang='stylus' scoped>
  .ratings
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      display flex
      padding 18px 0
      .overview-left
        flex 0 0 137px
        padding 6px 0
        width 137px
        border-right 1px solid rgba(7,17,27,0.1)
        text-align center
        @media only screen and (max-width: 320px)
          flex 0 0 120px
          width 120px
        .score
          margin-bottom 6px
          line-height 28px
          font-size 24px
          color rgb(255,153,0)
        .title
          margin-bottom 8px
          line-height 12px
          font-size 12px
          color rgb(7,17,27)
        .rank
          line-height 10px
          font-size 10px
          color rgb(147,153,159)
      .overview-right
        flex 1
        padding 6px 0 6px 24px
        padding-left 24px
        @media only screen and (max-width: 320px)
          padding-left 6px
        .score-wrapper
          margin-bottom 8px
          font-size 0
          .title
            display inline-block
            line-height 18px
            vertical-align top
            font-size 12px
            color rgb(7,17,27)
          .star
            display inline-block
            margin 0 12px
            vertical-align top
          .score
            display inline-block
            vertical-align top
            line-height 18px
            font-size 12px
            color rgb(255,153,0)
        .delivery-wrapper
          font-size 0
          .title
            line-height 18px
            font-size 12px
            color rgb(7,17,27)
          .delivery
            margin-left 12px
            font-size 12px
            color rgb(147,153,159)
    .rating-wrapper
      padding 0 18px
      .rating-item
        display flex
        padding 18px 0
        .avatar
          flex 0 0 28px
          width 28px
          margin-right 12px
          img 
            border-radius 50%
        .content
          position relative
          flex 1
          .name
            margin-bottom 4px
            line-height 12px
            font-size 10px
            color rgb(7,17,27)
          .star-wrapper
            margin-bottom 6px
            font-size 0
            .star
              display inline-block
              margin-right 6px
              vertical-align top
            .delivery
              display inline-block
              vertical-align top
              line-height 12px
              font-size 10px
              color rgb(147,153,159)
          .text
            margin-bottom 8px
            line-height 18px
            color rgb(7,17,27)
            font-size 12px
          .recommend
            line-height 16px
            font-size 0 
            .icon-arrow_lift,.item
              display inline-block
              margin 0 8px 4px 0
              font-size 9px
            .icon-arrow_lift
              color rgb(0,160,220 )
            .item
              padding 0 6px
              border-radius 1px
              color rgb(147,153,159)
              background #fff
          .time
            position absolute
            top 0
            right 0
            line-height 12px
            font-size 10px
            color rgb(147,153,159)

</style>