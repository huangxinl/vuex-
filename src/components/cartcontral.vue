<template>
    <div id="cart-contral">
        <span class = "decrease icon-remove_circle_outline" v-show = "food.count > 0" @click= "remove"></span>
        <span class = "count" v-show = "food.count > 0">{{food.count}}</span>
        <span class = "increase icon-add_circle" @click="add"></span>
    </div>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    // 绑定一个BScroll
    add (event) {
      if (event._constructed) {
        if (!this.food.count) {
          Vue.set(this.food, 'count')
          this.food.count = 1
        } else {
          this.food.count++
        }
      }
    },
    remove (event) {
      if (event._constructed) {
        this.food.count--
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='stylus' ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'

#cart-contral
    font-size 0
    .icon-remove_circle_outline , .icon-add_circle
        display inline-block
        width 24px
        height 24px
        line-height 24px
        font-size 24px
    .decrease , .increase
        color #00a0dc
        font-size 24px
        padding 6px
    .count
        display inline-block
        font-size 12px
        line-height 24px
        width 12px
        text-align center
        color rgb(147,153,159)
        padding 6px 0 6px 0
        vertical-align top
</style>
