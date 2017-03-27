<template>
  <div>
    <div class="header">
      <div class="content-wapper">
        <div class="avatar">
          <img :src="seller.avatar" alt="avatar">
        </div>
        <div class="content">
          <div class="title">{{seller.name}}</div>
          <div class="description">{{seller.description}}/{{seller.deliveryTime}}min arrive</div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text" >{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div class="support-count" v-if="seller.supports">
          <span class="count" @click="showDetail">{{seller.supports.length}}</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-wrapper" @click="showDetail">
        {{seller.bulletin}}
        <i class="icon-keyboard_arrow_right"></i>
      </div>
      <div class="background-header">
        <img :src="seller.avatar">
      </div>
    </div>
    <div class="detail" v-if="detailShow">
      <div class="detail-wrapper">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <star :size="48" :score="seller.score"></star>
        </div>
      </div>
      <div class="detail-close">
          <i class="icon-close"></i>
      </div>
      
    </div>
  </div>
</template>

<script>
import star from './../star/star.vue'

export default {
  name: 'header',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      classMap: [],
      detailShow: false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  methods: {
    showDetail () {
      this.detailShow = true
    }
  },
  components: {
    'star': star
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import './header.styl'
</style>
