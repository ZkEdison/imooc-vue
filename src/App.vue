<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="main-content">
      <router-view :seller="seller"></router-view>
    </div>
  </div>
</template>

<script>
import header from './components/header/header.vue'

const ERR_OK = 0

export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller')
          .then((res) => {
            if (res.data.errnor === ERR_OK) {
              this.seller = res.data.data
            }
          })
          .catch((error) => {
            console.log(error)
          })
  },
  components: {
    'v-header': header
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">

  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(#eee)
    .tab-item
      flex: 1
      text-align: center
      &>a
        display: block
        font-size: 14px
        color: rgb(77,85,93)
        &.active
          color: rgb(240,20,20)
  .main-content
    position: absolute
    top: 174px
    bottom: 0
    left: 0
    right: 0
</style>
