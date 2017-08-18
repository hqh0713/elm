<template>
	<div class="car-control">
		<div class="car-decrease" v-show="food.count>0" @click.stop.prevent="_descFoodNum"></div>
		<div class="car-count" v-show="food.count>0">{{food.count}}</div>
		<div class="car-add" @click.stop.prevent="_addFoodNum"></div>
	</div>
</template>

<script>
	export default{
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			_addFoodNum(event){
				if (!event._constructed) {
					return;
				}
				this.food.count++;
			},
			_descFoodNum(event){
				if (!event._constructed) {
					return;
				}
				this.food.count--;
				if (this.food.count <=0) {
					this.food.count=0;
				}
			}
		}
	};
</script>

<style>
.car-control{font-size: 0;}
.car-decrease{display: inline-block;background: url('car-desc.png') no-repeat;background-size: 24px 24px;width: 24px;height: 24px;line-height: 24px;}
.car-count{display: inline-block;font-size: 10px;line-height: 24px;color: rgb(147,153,159);padding: 0 12px;vertical-align: top;}
.car-add{display: inline-block;display: inline-block;width: 24px;height: 24px;line-height: 24px;background: url('car-add.png') no-repeat;background-size: 24px 24px;}
</style>
