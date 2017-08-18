<template>
   <div class="goods">
   	   <div class="menu-wrapper" ref="mscroll">
   	   	  <ul>
   	   	  	 <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index }" @click="selectMenu(index,$event)">
   	   	  	 	<span class="text border-1px">
   	   	  	 	   <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
   	   	  	 	</span>
   	   	  	 </li>
   	   	  </ul>
   	   </div>
       <div class="foods-wrapper" ref="fscroll">
       	  <ul>
       	  	 <li v-for="item in goods" class="food-list food-list-hook">
       	  	 	<h1 class="title">{{item.name}}</h1> 
       	  	 	<ul> 
       	  	 		<li v-for="food in item.foods"  @click="selectFood(food,$event)" class="food-item"> 
       	  	 			<div class="icon">
       	  	 				<img :src="food.icon">
       	  	 			</div>
       	  	 			<div class="content">
       	  	 				<h2 class="name">{{food.name}}</h2>
       	  	 				<p class="dec">{{food.description}}</p>
       	  	 				<div class="extra">
       	  	 					<span class="count">月售{{food.sellCount}}份</span>
       	  	 					<span>好评率{{food.rating}}%</span>
       	  	 				</div>
       	  	 				<div class="price">
       	  	 					<span class="now">￥{{food.price}}</span>
       	  	 					<span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
       	  	 				</div>
                    <div class="carControl-wrapper">
                      <car-control :food="food"></car-control>
                    </div>
       	  	 			</div>
       	  	 		</li>
       	  	 	</ul>
       	  	 </li>
       	  </ul>
       </div>
       <shop-car :seller="seller" :select-foods="selectFoods" :min-price="seller.minPrice" :delivery-price="seller.deliveryPrice"></shop-car> 
       <food :food="selectedFood" ref="food"></food>    
   </div>  
</template>

<script >
import BScroll from 'better-scroll'
import shopCar from '../shopCar/shopCar'
import carControl from '../carcontrol/carControl'
import food from '../food/food'

   export default {
       props:{
          seller:{
             type:Object
          }
       },
       data(){
       	  return {
            seller:[],
       	  	goods:[],
       	  	listHeight:[], /*存储右侧食品每个li的高度*/
       	  	scrollY:0,
            selectedFood:{}
       	  }
       },
       computed:{
       	  currentIndex(){
       	  	for (let i = 0; i < this.listHeight.length; i++) {
       	  		let height1 = this.listHeight[i];
       	  		let height2 = this.listHeight[i + 1];
       	  		if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
       	  			return i;
       	  		}
       	  	}
       	  	return 0;
       	  },
          selectFoods(){
            let foods=[];
            this.goods.forEach((good)=>{
              good.foods.forEach((food)=>{
                if (food.count) {
                   foods.push(food);
                }
              });
            });
            return foods;
          }
       },
       created(){
       	 this.classMap = ['decrease','discount','special','invoice','guarantee'];
         this.$http.get('/api/goods').then((response) =>{
         	this.goods = response.body.data;
         	this.$nextTick(() => {
                this._initScroll();
                this._calculateHeight();
         	});
         });
         this.$http.get('/api/seller').then((response) => {
            this.seller = response.body.data;
         });
       },
       methods:{
       	 _initScroll(){
       	 	this.mscroll = new BScroll(this.$refs.mscroll,{
       	 		click:true
       	 	});
       	 	this.fscroll = new BScroll(this.$refs.fscroll,{
            click:true,
       	 		probeType:3
       	 	});
       	 	this.fscroll.on('scroll',(pos) => {
       	 		this.scrollY =Math.abs(Math.round(pos.y));
       	 	});
       	 },
       	 _calculateHeight(){
       	 	let foodList = this.$refs.fscroll.getElementsByClassName('food-list-hook');
       	 	let height = 0;
       	 	this.listHeight.push(height);
       	 	for (let i = 0; i < foodList.length; i++) {
       	 		let item = foodList[i];
       	 		height += item.clientHeight;
       	 		this.listHeight.push(height);
       	 	}
       	 },
       	 selectMenu(index,event){
       	 	if (!event._constructed) {
       	 		return;
       	 	}
       	 	let foodList = this.$refs.fscroll.getElementsByClassName('food-list-hook');
       	 	let el = foodList[index];
       	 	this.fscroll.scrollToElement(el,300);
       	 },
         selectFood(food,event){
            if (!event._constructed) {
              return;
            }
            this.selectedFood = food;
            this.$refs.food.show();
         }
       },
       components:{
       	  'shop-car':shopCar,
          'car-control':carControl,
          'food':food
       }
    }
</script>

<style>
@media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3){
	.decrease{background-image: url("decrease_3@3x.png");}
	.discount{background-image: url("discount_3@3x.png");}
	.special{background-image: url("special_3@3x.png");}
	.invoice{background-image: url("invoice_3@3x.png");}
	.guarantee{background-image: url("guarantee_3@3x.png");}
}
@media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2){
	.decrease{background-image: url("decrease_3@2x.png");}
	.discount{background-image: url("discount_3@2x.png");}
	.special{background-image: url("special_3@2x.png");}
	.invoice{background-image: url("invoice_3@2x.png");}
	.guarantee{background-image: url("guarantee_3@2x.png");}
} 
.goods{display: flex; position: absolute;top: 174px;width: 100%;overflow: hidden;height: 100%;}/*若不设置height:100%,则better-scroll无法实现滚动*/

.menu-wrapper{flex: 0 0 80px;width: 80px;background:#f3f5f7;}
.menu-wrapper .current{margin-top: -1px;z-index: 10;background: #fff;font-weight: 700; }
.foods-wrapper{flex: 1;}
.menu-item{display: table;height: 54px;width: 56px;line-height: 14px;font-weight: 200;padding: 0 12px;}
.menu-item .icon{display: inline-block;width: 12px;height: 12px;background-size:12px 12px;background-repeat: no-repeat;}
.menu-item .text{display: table-cell;width: 56px;vertical-align: middle; font-size: 12px;}
h1.title{padding-left: 14px;height: 26px;line-height: 26px;border-left: 2px solid #d9dde1;font-size: 12px;color: rgb(147,153,159);background:#f3f5f7;}
.food-item{display: flex;margin: 18px;border-bottom: 1px solid rgba(7,17,27,0.2);padding-bottom: 18px;}
.food-item:last-child{border-bottom: none;margin-bottom: 0px;}
.food-item .icon{flex: 0 0 57px;}
.food-item .icon img{width: 57px;height: 57px;}
.food-item .content{flex: 1;position: relative;padding-left: 6px;}
.food-item .content .name{margin: 2px 0 8px 0;height: 14px;line-height: 14px;font-size: 14px;color: rgb(7,17,27);}
.food-item .content .dec,.extra{margin-bottom: 8px;line-height: 10px;font-size: 10px;color: rgb(147,153,159);}
.food-item .content .extra .count{margin-right: 12px;}
.food-item .content .price{font-weight: 700;line-height: 24px;}
.food-item .content .price .now{margin-right: 8px;font-size: 14px;color: rgb(240,20,20);}
.food-item .content .price .old{text-decoration: line-through;font-size: 10px;color: rgb(147,153,159);}
.carControl-wrapper{position: absolute;right: 0;bottom: 0px;}
</style>
