<template>
  <div class="shopcart">
   	<div class="content">
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
   		<div class="content-right">
   			<div class="pay" :class="payClass">{{payDesc}}</div>
   		</div>
   	</div>
  </div>
</template>

<script>
export default {
  name: 'shopcart',
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
        return [
          {price: 8, count: 2}
        ]
      }
    }
  },
  data () {
    return {
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylus">
	.shopcart
		position: fixed
		left: 0
		bottom: 0
		z-index: 50
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
