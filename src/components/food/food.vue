<template lang="html">
  <transition name="move">
    <div v-show = "showFlag" class="food" ref="food">
      <div>
        <div class="image-header">
          <img :src="food.image" alt="">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">${{food.price}}</span>
            <span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food='food'></cartcontrol>
          </div>
          <transition name="fade">
            <div class="buy" v-show="!food.count || food.count === 0" @click="addFirst">
              加入购物车
            </div>
          </transition>
        </div>
        <split v-show = "food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect></ratingselect>
        </div>
      </div>

    </div>
  </transition>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from './../cartcontr/cartcontrol.vue'
import Vue from 'vue'
import split from './../split/split.vue'
import ratingselect from './../ratingselect/ratingselect.vue'

export default {
  name: 'food',
  props: {
    food: {
      type: Object
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect
  },
  data () {
    return {
      showFlag: false
    }
  },
  created () {
  },
  methods: {
    show () {
      this.showFlag = true
      this.$nextTick(() => {
        console.log(this.$refs.food)
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    hide () {
      this.showFlag = false
    },
    addFirst (event) {
      if (!event._constructed) {
        return
      }
      console.log(event.target)
      this.$emit('addCart', event.target)
      Vue.set(this.food, 'count', 1)
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">

  .food
    position:fixed
    left: 0
    top: 0
    bottom: 48px
    right:0
    z-index: 30
    background-color:#fff
    &.move-enter-active, &.move-leave-active
      transition: all .8s
    &.move-enter, &.move-leave-active
      transform: translate3d(100%, 0, 0)
    .image-header
      position: relative
      width: 100%
      height:0
      padding-top: 100%
      img
        position:absolute
        top:0
        left: 0
        width: 100%
        height:auto
      .back
        position: absolute
        top: 10px
        left: 0
        .icon-arrow_lift
          display: block
          padding: 12px
          font-size: 20px
          color:#fff
    .content
      padding: 18px
      position: relative
      .title
        font-size:14px
        color:rgb(7, 17, 27)
        line-height: 14px
        margin-bottom: 8px
        font-weight: 700
      .detail
        margin-bottom: 18px
        line-height: 10px
        font-size: 0
        height: 10px
        .sell-count, .rating
          font-size: 10px
          color: rgb(147, 153, 159)
        .sell-count
          margin-right: 12px
      .price
        font-weight:700
        line-height:24px
        .now
          margin-right: 8px
          font-size: 14px
          color: rgb(240, 20, 20)
        .old
          text-decoration: line-through
          font-size: 10px
          color: rgb(147, 153, 159)
      .cartcontrol-wrapper
        position: absolute
        right: 18px
        bottom: 12px
      .buy
        position: absolute
        right: 18px
        bottom: 18px
        z-index: 10
        height: 24px
        line-height: 24px
        padding: 0 12px
        font-size: 10px
        color: #fff
        border-radius: 12px
        background-color:rgb(0, 160, 220)
        &.fade-enter-active, &.fade-leave-active
          transition:all .1s
        &.fade-enter, &.fade-leave
          opacity: 0
    .info
      padding: 18px
      .title
        line-height: 14px
        margin-bottom:6px
        font-size: 14px
        color: rgb(7, 17, 27)
      .text
        color:rgb(77, 83,93)
        font-size:12px
        padding: 0 8px
        line-height:24px

</style>
