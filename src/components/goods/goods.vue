<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref='aaa'>
        <ul>
          <li v-for="(item,index) in goods" :key="index" class="menu-item" :class="{'current':currentIndex === index}" @click="selectMenu(index)">
            <span class="text border-bottom">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref='bbb'>
        <ul>
          <li v-for="(item,index) in goods" class="food-lish food-list-hook" :key="index">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li @click="selectedfoodd(food)" v-for="(food,indes) in item.foods" class="food-item border-bottom" :key="indes">
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
                  <div class="cartcontrol-wraapper">
                    <cartcontrol :food='food'></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart :selectfoods='selectfoods' :delivery-price='seller.deliveryPrice' :min-price='seller.minPrice'></shopcart>
    </div>
    <food :food='selectedfood' ref='food'></food>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import shopcart from '../shopcart/shopcart'
import cartcontrol from '../cartcontrol/cartcontrol'
import food from '../food/food'
export default {
  name:'goods',
  data(){
    return {
      goods: [],
      scroll1:null,
      scroll2:null,
      listHeight:[],
      scrollY:0,
      selectedfood:{}
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
  computed: {
    currentIndex(){ //计算到达哪个区域的区间的时候的对应的索引值
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]; //当前menu子块的高度
          let height2 = this.listHeight[i + 1]; //下一个menu子块的高度
          //滚动到底部的时候,height2为undefined,需要考虑这种情况
          //需要确定是在两个menu子块的高度区间
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i; //返回这个menu子块的索引
          }
        }
        
        return 0; 
    },
    selectfoods(){
      let foods = []
      this.goods.forEach((good)=>{
        good.foods.forEach((food)=>{
          if(food.count){
            foods.push(food)
          }
        })
      })
      return foods
    }  
  },
  components:{
    shopcart,
    cartcontrol,
    food
  },
  methods: {
    selectMenu(index){

      let foodsList = this.$refs.bbb.getElementsByClassName('food-list-hook');
      
      let el = foodsList[index];
      // console.log(foodsList)
      // console.log(el)
        //类似jump to的功能,通过这个方法,跳转到指定的dom
      this.scroll2.scrollToElement(el, 300);
    },
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
      this.$nextTick(()=>{  // DOM更新之后获取子元素
        this._calculateHeight()
    })  
    },
    _calculateHeight(){
      let foodList = this.$refs.bbb.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]; //每一个item都是刚才获取的food的每一个dom
          height += item.clientHeight; //主要是为了获取每一个foods内部块的高度
          this.listHeight.push(height);
      }
    },
    selectedfoodd(food){
      this.selectedfood = food
      this.$refs.food.show()
    },
  },
  created(){
    this.classMap = ['decrease','discount','special','invoice','guarantee']
  },
  mounted() {
    this.getHomeInfo()
    
    // this.foodList = new BScroll(this.$refs.foolisthook,{})
//让页面挂载好后执行该函数
  
    this.scroll1 = new BScroll(this.$refs.aaa,{click:true})
    //this.$refs.aaa    ref=aaa
    this.scroll2 = new BScroll(this.$refs.bbb,{click:true,probeType: 3})
    this.scroll2.on('scroll', (pos) => { //事件的回调函数
          this.scrollY = Math.abs(Math.round(pos.y));//滚动坐标会出现负的,并且是小数,所以需要处理一下，实时取得scrollY
  })
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
        &.current
          position relative
          z-index 10
          margin-top -1px
          background #fff
          font-weight 700
          .text
            border none
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
          .cartcontrol-wraapper
            position absolute
            right 0
            bottom 12px

</style>