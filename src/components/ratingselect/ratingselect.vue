<template>
  <div class="ratingselect">
    <div class="rating-type border-bottom">
      <span @click="select(2)" class="block positive" :class="{'active':typename===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span @click="select(0)" class="block positive" :class="{'active':typename===0}">{{desc.positive}}<span class="count">{{positive.length}}</span></span>
      <span @click="select(1)" class="block negative" :class="{'active':typename===1}">{{desc.negative}}<span class="count">{{negative.length}}</span></span>
    </div>
    <div @click="toggleContent" class="switch border-bottom" :class="{'on':contentname}">
      <span class="icon-keyboard_arrow_right"></span>
      <span class="text">只看有内容的评论</span>
    </div>
  </div>
</template>

<script>

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
export default {
  name:'ratingselect',
  props:{
    ratings:{
      type:Array,
      default(){
        return []
      }
    },
    selectType:{
      type:Number,
      default:ALL
    },
    onlyContent:{
      type:Boolean,
      default:false
    },
    desc:{
      type:Object,
      default(){
        return {
          all:'全部',
          positive:'满意',
          negative:'不满意'
        }
      }
    }
  },
  data() {
    return {
      typename:this.selectType,
      contentname:this.onlyContent
    }
  },
  computed: {
    positive(){
      return this.ratings.filter((rating)=>{
        return rating.rateType === POSITIVE
      })
    },
    negative(){
      return this.ratings.filter((rating)=>{
        return rating.rateType === NEGATIVE
      })
    }
  },
  methods: {
    select(type){
      this.typename=type
      this.$emit('ratingtype',type)
      // this.$emit('ratingtype.select',type)
    },
    toggleContent(){
      this.contentname=!this.contentname
      this.$emit('ratingcontent',this.contentname)

      // this.$emit('ratingtype.select',type)
    }
  },
}
</script>

<style lang='stylus' scoped>
  .ratingselect
    .rating-type
      padding 18px 0
      margin 0 18
      font-size 0
      .block
        display inline-block
        padding 8px 12px
        margin-right 8px
        line-height 16px
        border-radius 1px
        font-size 12px
        color rgb(77,85,93)
        &active
          color #fff
        .count
          margin-left 2px
          font-size 8px
        &.positive
          background rgba(0,160,220,0.2)
          &.active 
            background rgb(0,160,220)
        &.negative
          background rgba(155,160,164,0.2)
          &.active
            background rgb(155,160,164)
    .switch
      padding 12px 18px
      line-height 24px
      color rgb(147,153,159)
      font-size 0
      &.on
        .icon-keyboard_arrow_right
          color #00c850
      .icon-keyboard_arrow_right
        display inline-block
        vertical-align top
        margin-right 4px
        font-size 24px
      .text
        font-size 12px
        display inline-block
        vertical-align top
</style>