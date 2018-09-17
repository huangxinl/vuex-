<template>
  <div id="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="" class="set-img">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand bg-image"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon bg-image"  :class="classMap[seller.supports[4].type]"></span>
          <span>{{seller.supports[4].description}}</span>
        </div>
        <div class="num-wrapper" v-if="seller.supports">
          <span class="num">{{seller.supports.length}}个</span>
          <span class="icon-keyboard_arrow_right more"></span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-icon"></span>
      <span class="bulletin">{{seller.bulletin}}</span>
    </div>
    <div class="background-wrapper">
      <img :src="seller.avatar" alt="">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice_1', 'guarantee']// 根据type匹配数组对应项
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" ref="stylesheet/stylus">

@import '../assets/stylus/index.styl'

#header
  position relative
  overflow hidden
  .content-wrapper
    position relative
    padding 24px 24px 18px
    background rgba(7,17,27,0.5)
    .avatar
      float left
    img
      border-radius 2px
      set-img(64px)
    .content
      margin-left 64px
      padding-left 16px
      color rgb(255,255,255)
      .title
        .brand
          margin-top 2px
          display inline-block
          width 30px
          height 18px
          vertical-align top
          bg-image('brand')
          background-size  30px 18px
        .name
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
      .description
        font-size 12px
        line-height 12px
        font-weight 200
        margin 8px 0px 10px 0px
      .support
        margin-bottom 2px
        font-size 12px
        span
          display inline-block
        .icon
          display inline-block
          vertical-align top
          width 16px
          height 16px
          background-size 16px 16px
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.guarantee
            bg-image('guarantee_1')
          &.invoice
            bg-image('invoice_1')
      .num-wrapper
        color rgb(255,255,255)
        position absolute
        right 12px
        bottom 14px
        padding 7px  8px
        font-size 10px
        border-radius 12px
        background-color rgba(0,0,0,0.2)
        height 12px
        line-height 12px
        .num
          font-size 10px
          line-height 10px
        .more
          width 2px
          vertical-align middle
        .icon-keyboard_arrow_right
          right 0px
          bottom 1px
          position relative
  .bulletin-wrapper
    position relative
    height 28px
    line-height 28px
    padding 0px 12px 0px 12px
    background-color rgba(7,17,27,0.2)
    overflow hidden
    text-overflow ellipsis
    white-space nowrap
    font-size 12px
    color white
    .bulletin-icon
      display inline-block
      vertical-align middle
      width 22px
      height 12px
      bg-image('bulletin')
      background-size 22px 12px
    .bulletin
      margin-left 4px
      margin-right 4px
      vertical-align middle
  .background-wrapper
    position absolute
    left 0
    top 0
    width 100%
    filter blur(10px)
    z-index -1
    img
     background-size cover
     width 100%
</style>
