<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count && food.count > 0" @click.stop.prevent="decreaseCart"></div>
    </transition >
    <div class="cart-count" v-show="food.count &&  food.count > 0">
      {{food.count}}
    </div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script>
  import Vue from 'vue'

  export default {
    name: 'cartcontrol',
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart (event) {
        console.log('start click')
        // if (!event._constructed) {
        //   return
        // }
        if (this.food.count === undefined) {
          console.log('count--undefined')
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
          this.$emit('addCart', event.target)
        }
        console.log('button' + this.food.count)
      },
      decreaseCart (event) {
        // if (!event._constructed) {
        //   return
        // }
        this.food.count > 0 && this.food.count--
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    color: rgb(0, 160, 220)
    .cart-decrease
      display:inline-block
      font-size:24px
      line-height: 24px
      padding: 6px
      &.move-enter-active, &.move-leave-active
        transition: all 0.4s
        opacity: 1
        transform: translate3D(0, 0,0) rotate(0deg)
      &.move-enter, &.move-leave-active
        opacity: 0
        transform: translate3D(24px, 0,0) rotate(180deg)
    .cart-count
      display:inline-block
      font-size: 12px
      width: 12px
      padding-top: 6px
      line-height:24px
      text-align: center
      color: rgb(147, 153, 159)
      vertical-align: top
    .cart-add
      display:inline-block
      font-size:24px
      line-height: 24px
      padding: 6px

</style>
