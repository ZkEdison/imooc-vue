<template>
  <div class="shopcart">
   	<div class="content" @click.stop.prevent="toggleList">
   		<div class="content-left">
   			<!-- icon -->
   			<div class="logo-wrapper">
   				<div class="logo" :class="{'highlight': totalCount > 0}">
   					<span class="icon-shopping_cart"> </span>
   				</div>
          <div class="num" v-show="totalCount > 0">
            {{totalCount}}
          </div>
   			</div>
   			<!-- price -->
        <div class="price" :class="{'highlight': totalCount > 0}">￥{{totalPrice}}</div>
        <!-- desc -->
        <div class="desc">另需配送费￥{{deliveryPrice}} 元</div>
   		</div>
   		<div class="content-right" @click.stop.prevent="pay">
   			<div class="pay" :class="payClass">{{payDesc}}</div>
   		</div>
   	</div>
    <div class="ball-container">
      <div v-for="ball in balls">
        <transition name="drop"
                    v-on:before-enter="dropBeforeEnter"
                    v-on:enter="dropEnter"
                    v-on:after-enter="dropAfterEnter">
          <div class="ball" v-show="ball.show">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </div>
    </div>
    <transition  name="fold">
      <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">ShopCar</h1>
            <span class="empty" @click="empty">Clear</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul class="">
              <li class="food" v-for = "food in selectFoods">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span class="">${{food.price*food.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
      </div>
    </transition >
    <transition name='fade'>
      <div class="list-mask" v-show="listShow" @click="hideList">

      </div>
    </transition>

  </div>
</template>

<script>
import cartcontrol from './../cartcontr/cartcontrol'
import BScroll from 'better-scroll'

export default {
  name: 'shopcart',
  components: {
    cartcontrol
  },
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
    },
    selectFoods: {
      type: Array,
      default () {
        return
      }
    }
  },
  data () {
    return {
      fold: true,
      balls: [
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        }
      ],
      dropBalls: []
    }
  },
  methods: {
    drop (el) {
      console.log('shopcart 组件传递进来的' + el)
      console.log(el)
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = el
          this.dropBalls.push(ball)
          console.log(this.balls)
          console.log(this.dropBalls)
          return
        }
      }
    },
    // transition
    dropBeforeEnter (el) {
      console.log('drop-before-enter')
      console.log(el)
      let count = this.balls.length
      while (count--) {
        let bull = this.balls[count]
        if (bull.show) {
          let rect = bull.el.getBoundingClientRect()
          let x = rect.left - 32
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0, ${y}px, 0)`
          el.style.transform = `translate3d(0, ${y}px, 0)`
          console.log(x + ':--:' + y)

          let inner = el.getElementsByClassName('inner-hook')[0]
          inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`
          inner.style.transform = `translate3d(${x}px, 0, 0)`
        }
      }
    },
    dropEnter (el, done) {
      /* eslint-disable */
      console.log('dropEnter')
      /* 触发******** */
      let rf = el.offsetHeight
      console.dir(el.offsetHeight)
      this.$nextTick(() => {
        el.style.display = ''
        el.style.webkitTransform = 'translate3d(0, 0, 0)'
        el.style.transform = 'translate3d(0, 0, 0)'

        let inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translate3d(0, 0, 0)'
        inner.style.transform = 'translate3d(0, 0, 0)'
        el.addEventListener('transitionend', done)
      })
    },
    dropAfterEnter (el) {
      console.log('dropAfterEnter')
      let ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = 'none'
      }
    },
    toggleList () {
      if (!this.totalCount) {
          return
      }
      this.fold = !this.fold
      console.log(`this.fold:${this.fold}`)
    },
    empty () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    },
    hideList () {
      this.fold = !this.fold
    },
    pay () {
      if (this.totalPrice < this.minPrice) {
        return
      }
      window.alert('pay' + this.totalPrice)
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice >= this.minPrice) {
        return 'enouth'
      }
    },
    listShow () {
      if (!this.totalCount) {
           this.fold = true
           return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus">
@import './shopcart.styl'
  .shopcart
    position: fixed
    left: 0
    bottom: 0
    z-index: 501
    width: 100%
    height: 48px
    .content
      display: flex
      .content-left
        flex:1
        height: 48px
        background: rgb(20, 29, 39)
        font-size: 0
        .logo-wrapper
          display: inline-block
          position: relative
          top:-10px
          margin: 0 12px
          padding:6px
          height:56px
          width: 56px
          box-sizing: border-box
          vertical-align: top
          border-radius: 100%
          background: rgb(20, 29, 39)
          .logo
            width: 100%
            height: 100%
            border-radius: 100%
            background: #2b3c3c
            text-align: center
            color:#80858a
            &.highlight
              background: rgb(0,160,220)
              color: #fff
            span
              font-size:24px
              line-height: 44px
          .num
            position: absolute
            top: 0
            right:0
            width: 24px
            height: 16px
            line-height: 16px
            text-align:center
            border-radius: 16px
            background: rgb(240,20,20)
            box-shadow: 0 4px 8px rgba(0,0,0,0.4)
            color: #fff
            font-size:12px
        .price
          display: inline-block
          vertical-align: top
          line-height: 24px
          margin-top: 12px
          box-sizing: border-box
          padding-right: 12px
          border-right: 1px solid rgba(255,255,255,0.1)
          font-size: 16px
          font-weight: 700
          color: rgba(255, 255,255 ,0.4)
          &.highlight
            color:#fff
        .desc
          display: inline-block
          vertical-align: top
          line-height: 24px
          margin: 12px 0 0 12px
          color: rgba(255,244,244,0.4)
          font-size: 14px
      .content-right
        flex: 0 0 105px
        height: 48px
        background: #333
        .pay
          line-height: 48px
          font-size: 14px
          text-align: center
          color: rgba(255, 255, 255, 0.4)
          &.enouth
           background: #00b34c
           color: #fff
</style>
