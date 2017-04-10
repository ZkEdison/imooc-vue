<template>
  <div class="goods">
  	<!-- left -->
   	<div class="menu-wrapper" ref="menuWrapper">
   		<ul class="list">
   			<li v-for="(item,index) in goods" class="menu-item border-1px"
   			:class="{'current': index === currentIndex }"
   			@click="selectMenu(index, $event)">
   				<span class="text">
   					<span v-show="item.type >= 0" class=icon :class="classMap[item.type]"></span>
   					{{item.name}}
   				</span>
   			</li>
   		</ul>
   	</div>
   	<!-- right -->
   	<div class="foods-wrapper" ref="foodsWrapper">
   		<ul>
   			<li v-for="(item,index) in goods"  ref="foodList">
   				<h1 class="title">
   					{{index}}{{item.name}}
   				</h1>
   				<div v-for="food in item.foods"
   					class="food-item border-1px" @click="selectFood(food, $event)">
   					<div class="icon">
   						<img :src="food.icon">
   					</div>
   					<div class="content">
   						<h2 class="name">{{food.name}}
   						</h2>
   						<p class="desc">
   							{{food.description}}
   						</p>
   						<div class="extra">
   							<span class="count">yueshou {{food.sellCount}}
   							&nbsp;
   							haoping {{food.rating}}
   							</span>
   						</div>
   						<div class="price">
   							<span class="now">
   								${{food.price}}
   							</span>
   							<span v-show="food.oldPrice" class="old">
   								{{food.oldPrice}}
   							</span>
   						</div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food" @addCart="addCart"></cartcontrol>
              </div>
   					</div>
   				</div>
   			</li>
   		</ul>
   	</div>
   	<shopcart ref="shopCartElement" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectFoods"></shopcart>
    <food :food="selectFoods" ref="food"></food>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from '../shopcart/shopcart'
import cartcontrol from './../cartcontr/cartcontrol'
import food from './../food/food.vue'

const ERR_OK = 0
export default {
  name: 'goods',
  props: {
  	seller: {
  		type: Object
  	}
  },
  components: {
  	shopcart,
    cartcontrol,
    food
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  created () {
    this.$http.get('/api/goods')
     .then((res) => {
       if (res.data.errnor === ERR_OK) {
         this.goods = res.data.data
         this.$nextTick(() => {
         	this._initScroll()
         	this._calculateHeight()
         })
       }
     })
     .catch((error) => {
       console.log(error)
     })
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  },
  watch: {
  },
  methods: {
 		_initScroll	() {
 			// console.log(this.$refs)
 			// console.log(this.$refs.menuWrapper)
 			this.munuScroll = new BScroll(this.$refs.menuWrapper, {
 				click: true
 			})
 			this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
 				probeType: 3, click: true
 			})
 			this.foodsScroll.on('scroll', (pos) => {
 				this.scrollY = Math.abs(Math.round(pos.y))
  		})
 		},
 		_calculateHeight () {
 			let foodList = this.$refs.foodList
 			console.log(foodList)
 			let height = 0
 			this.listHeight.push(height)
 			if (foodList) {
 				for (let i = 0; i < foodList.length; i++) {
 				let item = foodList[i]
 				height += item.clientHeight
 				this.listHeight.push(height)
 				}
 			}
 		},
 		selectMenu (index, event) {
 			console.log('click')
 			if (!event._constructed) {
 				return
 			}
 			let foodList = this.$refs.foodList
 			let el = foodList[index]
 			this.foodsScroll.scrollToElement(el, 100)
 			console.log(index)
 		},
    _drop (target) {
      this.$refs.shopCartElement.drop(target)
    },
    addCart (target) {
      // if (!event._constructed) {
      //   return
      // }
      this._drop(target)
    },
    selectFood (food, event) {
      this.selectedFood = food
      this.$refs.food.show()
    }
  },
  computed: {
  	currentIndex: function () {
    		for (let i = 0; i < this.listHeight.length; i++) {
  				let height1 = this.listHeight[i]
  				let height2 = this.listHeight[i + 1]
  				if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
  					return i
  				}
  			}
  			return 0
  	},
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((good) => {
          if (good.count) {
            foods.push(good)
          }
        })
      })
      return foods
    }
   // currentIndex: function () {
   //  	for (let i = 0; i < this.listHeight.length; i++) {
   //    	let height1 = this.listHeight[i]
   //     	let height2 = this.listHeight[i + 1]
   //    	if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
   //      	console.log(i)
   //      	return i
   //    	}
   //  	}
   //    	return 0
   // 	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
		@import "./goods.styl"
</style>
