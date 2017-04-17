<template>
  <div class="ratings" ref="ratingsEle">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">
            高于周边商家{{seller.rankRate}}%
          </div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect :selectType="selectType"
                    :onlyContent="onlyContent"
                    :desc="desc"
                    :ratings="ratings"
                    @ratingtypeSelect="ratingtypeChild"
                    @contentToggle="contentToggleChild">
      </ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li class="rating-item" v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" alt="" width="28" height="28">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span v-for="item in rating.recommend" class="item">{{item}}</span>
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
import star from './../star/star.vue'
import split from './../split/split.vue'
import ratingselect from './../ratingselect/ratingselect.vue'
import {formatDate} from './../../common/js/date.js'
import BScroll from 'better-scroll'
import dataJson from './../../../data.json'

// const ERR_OK = 0
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
console.log(POSITIVE, NEGATIVE, ALL)

export default {
  name: 'ratings',
  data () {
    return {
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      },
      ratings: []
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  components: {
    star,
    split,
    ratingselect
  },
  created () {
    this.ratinngs = dataJson.ratings
    this.$nextTick(() => {
      this.scroll = new BScroll(this.$refs.ratingsEle, {
        click: true
      })
    })
    // this.$http.get('./api/ratings').then((res) => {
    //   if (res.data.errnor === ERR_OK) {
    //     this.ratings = res.data.data
    //     this.$nextTick(() => {
    //       this.scroll = new BScroll(this.$refs.ratingsEle, {
    //         click: true
    //       })
    //     })
    //   }
    // })
  },
  computed: {
  },
  methods: {
    ratingtypeChild (type) {
      console.log('响应ratingtypeSelect' + type)
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    contentToggleChild () {
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylsu">
  @import "./../../common/stylus/mixin.styl"
  .ratings
    position: absolute
    top: 0
    bottom: 0
    left: 0
    width: 100%
    overflow: hidden
    .overview
      display: flex
      padding: 18px 0
      .overview-left
        flex:0 0 137px
        width: 137px
        border-right: 1px solid rgba(7, 17, 27, .1)
        text-align: center
        padding:6px 0
        @media only screen and (max-width: 320px)
          flex: 0 0 120px
          width: 120px
        .score
          line-height: 28px
          font-size:24px
          color: rgb(255, 153, 0)
          margin-bottom: 6px
        .title
          line-height: 12px
          margin-bottom: 8px
          font-size: 12px
          color:rgb(7, 17, 27)
        .rank
          color:rgba(7, 17, 27, .7)
          font-size:10px
          line-height:10px
      .overview-right
        flex: 1
        padding: 6px 0 6px 24px
        @media only screen and (max-width: 320px)
          padding-left: 6px
        .score-wrapper
          margin-bottom: 8px
          font-size: 0
          .title
            line-height:18px
            font-size: 12px
            display: inline-block
            vertical-align: top
            color: rgb(7, 17 ,27)
          .star
            display: inline-block
            vertical-align: top
            margin: 0 12px
          .score
            display: inline-block
            line-height:18px
            vertical-align: top
            font-size: 12px
            color: rgb(255, 153, 0)
        .delivery-wrapper
          font-size: 0
          .title
            line-height: 18px
            font-size: 12px
            color:rgb(7, 17, 27)
          .delivery
            font-size:12px
            color:rgb(147, 153, 159)
            margin-left:15px
    .rating-wrapper
      padding: 0 18px
      .rating-item
        display:flex
        padding:18px 0
        border-1px(rgba(7, 17, 27, .1))
        .avatar
          flex: 0 0 28px
          width: 28px
          margin-right: 12px
          img
            border-radius:100%
        .content
          flex: 1
          position:relative
          .name
            line-height:12px
            font-size: 10px
            color:rgb(7, 17, 27)
            margin-bottom:4px
          .star-wrapper
            margin-bottom:6px
            font-size:0
            text-align:left
            .star
              display:inline-block
              vertical-align:top
              margin-right:6px
            .delivery
              display:inline-block
              vertical-align:top
              line-height:12px
              font-size: 10px
              color:rgb(7, 17, 27)
          .text
            margin-bottom:8px
            line-height: 18px
            color:rgb(7, 17, 27)
            font-size:12px
          .recommend
            line-height:16px
            .icon-thumb_up, .item
              display:inline-block
              margin: 0 8px 4px 0
              font-size: 9px
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .item
              padding: 0 6px
              border:1px solid rgba(7, 17, 27, .1)
              border-radius:1px
              color:rgb(147, 153, 159)
              background-color:#fff
          .time
            position:absolute
            top: 0
            right:0
            line-height:12px
            font-size:10px
            color:rgb(147, 153, 159)

</style>
