<template>
  <div id="goods">
     <div class="menu-wrapper" ref="menuWrapper">
       <div class="menu-item"  :class="{current : currentIndex === index}" v-for="(item,index) in goods" :key="index" @click = "selectIndex(index,$event)">
         <div class="text-wrapper    border-1px" >
           <span class="icon" v-show="item.type > 0" :class="classMap[item.type]"></span>
           <span class="text">{{item.name}}</span>
         </div>
       </div>
     </div>
     <div class="foods-wrapper" ref="foodsWrapper">
       <ul>
         <li v-for="(item1,index) in goods" :key="index" class="food-list-hook">
           <div class="items-title">{{item1.name}}</div>
           <ul>
             <li  v-for="(item2,index) in item1.foods" :key="index" class="food-name">
                <div class="item-icon">
                  <img :src="item2.icon" alt="">
                </div>
                <div class="item-content border-1px">
                  <div class="cont-name">{{item2.name}}</div>
                  <div class="description">{{item2.description}}</div>
                  <div class="ratings">
                    <span class="sellcount">月售{{item2.sellCount}}份</span>
                    <span class="rating">{{item2.rating}}%好评率</span>
                  </div>
                  <div class="price">
                    <span class="current-price">￥{{item2.price}}</span>
                    <span class="old-price" v-show="item2.oldPrice">￥{{item2.oldPrice}}</span>
                  </div>
                  <div class="cart-contral-wrapper">
                    <cartcontral :food = "item2"></cartcontral>
                  </div>
                </div>
             </li>
           </ul>
         </li>
       </ul>
     </div>
       <shopcart :deliveryPrice = "seller.deliveryPrice" :minPrice= "seller.minPrice" :selectFoods = "selectFoods"></shopcart>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'
import BScroll from 'better-scroll'
import shopcart from './shopcart'
import cartcontral from './cartcontral'// 引入组件 并注册

export default {
  name: 'goods',
  data () {
    return {
      goods: [],
      heightList: [],
      scrollY: 0
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    shopcart,
    cartcontral
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice_1', 'guarantee']
    axios.get('/good/goods').then(res => {
      if (res.data.code === 0) {
        this.goods = res.data.data
        // console.log(res.data.data)
        Vue.nextTick(() => {
        // 计算每个food-list-hock高度
          this._initScroll()
          this._caculateHeight()
        // dom绑定一个scroll
        })
      }
    })
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.heightList.length; i++) {
        let height1 = this.heightList[i]
        let height2 = this.heightList[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () { // 这个函数的返回值为一个数组 通过（:selectFoods = "selectFoods"）这种方法传入购物车
      let foods = []
      if (this.goods.length !== 0) {
        this.goods.forEach(good => {
          good.foods.forEach(food => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
      }
      return foods
    }
  },
  methods: {
    selectIndex ($index, $event) {
      if (!$event._constructed) {
        return
      }
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      this.foodScroll.scrollToElement(foodList[$index], 300)
    },
    //  better-scroll
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
        probeType: 3,
        click: true
      })
      this.foodScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _caculateHeight () {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.heightList.push(height)
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.heightList.push(height)
      }
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='stylus' ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'

#goods
  display flex
  position absolute
  top 178px
  bottom 48px
  left 0
  right 0
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 80px
    width 80px
    background-color #f3f5f7
    .menu-item
      font-size 0
      line-height 16px
      padding 0 12px
      &.current
        position relative
        margin-top -1px
        background #ffffff
      &.after
        border-top 1px solid #ffffff
      .text
        font-size 12px
        line-height 14px
        font-weight 500
    .text-wrapper
      display table-cell
      height 54px
      width 56px
      border-1px rgba(7,17,27,0.1)
      vertical-align middle
      .text
        font-size 12px
        line-height 14px
        font-weight 200
      .icon
        display inline-block
        width 14px
        height 14px
        background-size 14px 14px
        vertical-align top
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
  .foods-wrapper
    flex 1
    .food-list-hook
      background-color #f3f5f7
      .items-title
        padding 10px
        border-left 4px rgba(7,17,27,0.1) solid
      .food-name
        padding 18px
        display flex
        background rgb(255,255,255)
        border-1px rgba(7,17,27,0.1)
      img
        width 78px
        height 78px
        display inline-block
        border-radius 4px
      .item-content
         margin-left 10px
        .cont-name
          font-size 14px
          color rgb(7,17,27)
          line-height 14px
          margin-top 2px
        .description
          font-size 10px
          color rgb(147,153,159)
          line-height 14px
          padding 8px 0px
        .sellcount,.rating
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
      .price
        padding-top 8px
        .current-price
          font-size 14px
          color #f40
          font-weight 700
        .old-price
          font-size 10px
          text-decoration line-through
          font-weight normal
          color rgb(147,153,159)
      .cart-contral-wrapper
        position absolute
        right 10px
        bottom 20px
</style>
