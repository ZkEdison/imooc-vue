<template lang="html">
  <div class="ratingselect">
    <div class="rating-type border-1px">
      <span class="block positive" :class="{'active': selectType === 2}" @click.stop.prevent="select(2, $event)">
        {{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span class="block positive" :class="{'active': selectType === 0}" @click.stop.prevent="select(0, $event)">
        {{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span class="block negative" :class="{'active': selectType === 1}" @click.stop.prevent="select(1, $event)">
        {{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch" :class="{'on':onlyContent}" @click.stop.prevent="toggleContent">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
console.log(POSITIVE + NEGATIVE)
export default {
  name: 'ratingselect',
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  computed: {
    positives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE
      })
    },
    negatives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE
      })
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return
      }
      console.log('触发ratingtypeSelect')
      // this.selectType = type
      this.$emit('ratingtypeSelect', type)
    },
    toggleContent (event) {
      if (!event._constructed) {
        return
      }
      console.log('contentToggle')
      // this.onlyContent = !this.onlyContent
      this.$emit('contentToggle')
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import './../../common/stylus/mixin.styl'
  .ratingselect
    .rating-type
      padding: 18px 0
      margin: 0 18px
      border-1px(rgba(7, 17, 27, .1))
      font-size:0
      .block
        display:inline-block
        padding: 8px 12px
        margin-right: 8px
        border-radius: 1px
        color: rgb(77, 85, 93)
        font-size: 12px
        line-height: 16px
        &.active
          color:#fff
        .count
          font-size: 8px
          margin-left:2px
        &.positive
          background-color: rgba(0, 160, 220, .2)
          &.active
            background-color: rgba(0, 160, 220, 1)
        &.negative
          background-color: rgba(77, 85, 93, .2)
          &.active
            background-color: rgb(77, 85, 93)
    .switch
      padding: 12px 18px
      line-height: 24px
      border-bottom: 1px solid rgba(7, 17, 27, .1)
      color: rgb(147, 153, 159)
      font-size: 0
      &.on
        .icon-check_circle
          color: #00c850
      .icon-check_circle
        display:inline-block
        font-size: 22px
        margin-right: 4px
        vertical-align:top
      .text
        font-size: 12px
        display:inline-block
        vertical-align:top
</style>
