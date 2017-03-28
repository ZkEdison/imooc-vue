<template>
  <div class="image-container"> 
    <div class="image-wrap">
      <img :src="preImageSrc" 
            @touchstart="addTouchStart"
            @touchmove="addTouchMove"
            @touchend="addTouchEnd">
    </div>
  </div>
</template>

<script>
export default {
  name: 'imageScale',
  props: {
    preImageSrc: {
      type: String
    }
  },
  data () {
    return {
      distX: 0,
      distY: 0,
      newX: 0,
      newY: 0
    }
  },
  created () {
  },
  methods: {
    addTouchStart (e) {
      e.preventDefault()
      let target = e.target
      let touchList = e.changedTouches[0]
      /* 当前手指的坐标 distx 和 disty */
      this.distX = e.changedTouches[0].clientX
      this.distY = e.changedTouches[0].clientY

      console.log(target)
      console.log(`x:${this.distX},y:${this.distY}`)

      if (e.targetTouches.length === 1) {
        console.log(1)
        console.log(touchList)
      } else {
        console.log(2)
      }
    },

    addTouchMove (e) {
      e.preventDefault()
      /* 当前手指的坐标 distx 和 disty */
      this.newX = e.changedTouches[0].clientX
      this.newX = e.changedTouches[0].clientY
      console.log(`x:${this.distX},y:${this.distY}`)
      let x = this.newX - this.distX
      let y = this.newY - this.distY
      /* 单手指移动 */
      if (e.targetTouches.length === 1) {
        this.move(e, x, y)
      }
    },

    addTouchEnd (e) {
      let target = e.target
      console.log(target)
    },

    move (e, x, y) {
      e.target.style.transform = `translate3d(${x}px,${y}px,0)`
      console.log(`translate3d(${x}px,${y}px,0)`)
    }
  },
  components: {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  .image-container
    position:fixed
    top:0
    bottom:0
    left:0
    right:0
    z-index:999999
    background-color:#000
    .image-wrap
      width: 100%
      position: absolute
      top:50%
      left:50%
      transform:translate3d(-50%,-50%,0)
      text-align:center
      img
        width: 80%
        
        
</style>
