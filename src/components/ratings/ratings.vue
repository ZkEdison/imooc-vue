<template>
  <div class="ratings">
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
      <ratingselect></ratingselect>
    </div>
  </div>
</template>

<script>
import star from './../star/star.vue'
import split from './../split/split.vue'
import ratingselect from './../ratingselect/ratingselect.vue'

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
      }
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star,
    split,
    ratingselect
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylsu">
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
</style>
