<template>
  <div class="goods">
  	<!-- left -->
   	<div class="menu-wrapper">
   		<ul class="list">
   			<li v-for="item in goods" class="menu-item border-1px" >
   				
   				<span class="text">
   					<span v-show="item.type >= 0" class=icon :class="classMap[item.type]"></span>
   					{{item.name}}
   				</span>
   			</li>
   		</ul>
   	</div>
   	<!-- right -->
   	<div class="foods-wrapper">
   		<ul>
   			<li v-for="item in goods">
   				<h1 class="title">
   					{{item.name}}
   				</h1>
   				<div v-for="food in item.foods"
   					class="food-item border-1px">
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
   							<span class="count">yueshou {{food.sellCount}} &nbsp;
   							haoping {{food.rating}}</span>
   						</div>
   						<div class="price">
   							<span class="now">
   								${{food.price}}
   							</span>
   							<span v-show="food.oldPrice" class="old">
   								{{food.oldPrice}}
   							</span>
   						</div>
   					</div>
   				</div>
   			</li>
   		</ul>
   	</div>
  </div>
</template>

<script>
const ERR_OK = 0
export default {
  name: 'goods',
  data () {
    return {
      goods: {}
    }
  },
  created () {
    this.$http.get('/api/goods')
     .then((res) => {
       if (res.data.errnor === ERR_OK) {
         this.goods = res.data.data
       }
     })
     .catch((error) => {
       console.log(error)
     })
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
		@import "./goods.styl"
</style>
