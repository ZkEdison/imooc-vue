<template lang="html">
  <transition name="move">
    <div v-show = "showFlag" class="food-detail" ref="food">
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
            <cartcontrol :food='food' @addCart="addCart"></cartcontrol>
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
          <ratingselect :selectType='selectType'
                        :onlyContent='onlyContent'
                        :desc="desc"
                        :ratings="food.ratings"
                        @ratingtypeSelect="selectTypeChild"
                        @contentToggle='onlyContentChild'>
          </ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-for="rating in food.ratings" class="rating-item" v-show="needShow(rating.rateType, rating.text)">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img :src="rating.avatar" alt="" class="avatar" width="12" height="12">
                </div>
                <div class="time">
                  {{rating.rateTime | formatDate}}
                </div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType === 0, 'icon-thumb_down':rating.rateType === 1}"></span>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
              暂无评价
            </div>
          </div>
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
import {formatDate} from './../../common/js/date.js'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
console.log(POSITIVE, NEGATIVE, ALL)
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
      showFlag: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  created () {
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods: {
    show () {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = true
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
    },
    addCart () {
      this.$emit('addCart', event.target)
    },
    selectTypeChild (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    onlyContentChild () {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import './../../common/stylus/mixin.styl'

  .food-detail
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
    .rating
      padding-top: 18px
      .title
        line-height: 14px
        font-size:14px
        color: rgb(7, 17, 27)
        margin-left: 18px
      .rating-wrapper
        padding: 0 18px
        .rating-item
          position: relative
          padding: 16px 0
          border-1px(rgba(7, 17, 27, .1))
          .user
            position: absolute
            right: 0
            top: 16px
            font-size:0
            line-height:12px
            .name
              display: inline-block
              vertical-align: top
              font-size: 10px
              color:rgb(147, 153, 159)
              margin-right: 6px
            .avatar
              border-radius: 50%
          .time
            line-height: 16px
            font-size: 10px
            color: rgb(147, 153, 159)
            margin-bottom:6px
          .text
            line-height:16px
            font-size: 12px
            color: rgb(7, 17, 27)
            .icon-thumb_up, .icon-thumb_down
              margin-right: 4px
              font-size: 12px
              line-height:16px
            .icon-thumb_up
              color: rgb(0, 160 ,220)
            .icon-thumb_down
              color: rgb(147, 153, 159)
        .no-rating
          padding: 16px 0
          font-size: 12px
          color:rgb(147, 153 159)


</style>
