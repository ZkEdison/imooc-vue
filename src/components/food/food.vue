<template lang="html">
  <transition name="move">
    <div v-show = "showFlag" class="food">

    </div>
  </transition>
</template>

<script>
export default {
  name: 'food',
  porps: {
    food: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false
    }
  },
  methods: {
    show () {
      this.showFlag = true
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
</style>
