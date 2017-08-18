<template>
   <transition name="move">
   	  <div class="food" v-show="showFlag" ref="food" >
   	     <div class="food-wrapper">
   	     	<div class="image-header">
   	     		<img :src="food.image">
   	     		<div class="back" @click="hide">
   	     			<i class="go-back"></i>
   	     		</div>
   	     	</div>
   	     	<div class="food-content">
   	     	    <h1 class="food-title">{{food.name}}</h1>
   	     	    <div class="food-detail">
   	     			<span class="sell-count">月售{{food.sellCount}}份</span>
   	     			<span class="rating">好评率{{food.rating}}</span>
   	     		</div>
   	     		<div class="price">
       	  	 		<span class="now">￥{{food.price}}</span>
       	  	 		<span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
       	  	 	</div>
       	  	 	<div class="carcontrol-wrapper">
   	     		    <car-control :food="food"></car-control>
   	     	    </div>
   	     	     <div class="buy" v-show="!food.count || food.count===0" @click.stop.prevent="addFirst">加入购物车</div>
   	     	</div> 
   	     	<split v-show="food.info"></split>
   	     	<div class="info" v-show="food.info">
   	     		<h1 class="info-title">商品信息</h1>
   	     		<p class="info-text">{{food.info}}</p>
   	     	</div>
   	     	<split></split>
   	     	<div class="rating">
   	     		<h1 class="rating-title">商品评价</h1>
   	     		<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
   	     		<div class="rating-wrapper">
   	     			<ul v-show="food.ratings && food.ratings.length">
   	     				<li v-for="rating in food.ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
   	     					<div class="user">
   	     						<span class="user-name">{{rating.username}}</span>
   	     						<img class="user-avatar" width="12" height="12" :src="rating.avatar">
   	     					</div>
   	     					<div class="user-time">{{rating.rateTime | formatDate}}</div>
   	     					<p class="user-text">
   	     						<span :class="{'user_up':rating.rateType===0,'user_down':rating.rateType===1}"></span>
   	     						{{rating.text}}
   	     					</p>
   	     				</li>
   	     			</ul>
   	     			<div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
   	     		</div>
   	     	</div>
   	     </div>
   	  </div>
   </transition>	
</template>

<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import carControl from '../carcontrol/carControl'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
import {formatDate} from '../../common/js/date'

const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;


export default{
	props:{
		food:{
			type:Object
		}
	},
	data(){
		return{
			showFlag:false,
			selectType:ALL,
			onlyContent:false,
			desc:{
				all:'全部',
				positive:'推荐',
				negative:'吐槽'
			}
		}
	},
	methods:{
		show(){
			this.showFlag = true;
			this.selectType = ALL;
			this.onlyContent = true;
			this.$nextTick(()=>{
				if (!this.scroll) {
					this.scroll = new BScroll(this.$refs.food,{
						click:true
					});
				}else{
					this.scroll.refresh();
				}
			});
		},
		hide(){
			this.showFlag = false;
		},
		addFirst(event){
			if (!event._constructed) {
				return;
			}/*防止在pc端产生多次点击事件*/
			Vue.set(this.food,'count',1	)
		},
		needShow(type,text){
			if (this.onlyContent && !text) {
				return false;
			}
			if (this.selectType===ALL) {
				return true;
			}else{
				return type===selectType;
			}
		}
	},
	events:{
		'ratingtype.select'(type) {
			this.selectType = type;
			this.$nextTick( () => {
				this.scroll.refresh();
			});
		},
		'content.toggle'(onlyContent) {
			this.onlyContent=onlyContent;
			this.$nextTick( () => {
				this.scroll.refresh();
			});
		}
	},
	filters:{
		formatDate(time){
			let date =new Date(time);
			return formatDate(date,'yyyy-MM-dd hh:mm');
		}
	},
	components:{
		'car-control':carControl,
		'split':split,
		'ratingselect':ratingselect
	}
};
</script>

<style>
.food{position: fixed;left: 0;top: 0;bottom: 48px;z-index: 15;width: 100%;background-color: #fff;}
.move-enter-active{transition: all 0.2s linear;transform: translate3d(0,0,0);}
.move-enter,.move-leave-active{transform: translate3d(100%,0,0);transition: all 0.2s linear;}
.image-header{position: relative;width: 100%;height: 0;padding-top: 100%;}
.image-header img{position: absolute;top: 0;left: 0;width: 100%;height: 100%;}
.back{position: absolute;top: 10px;left: 0;background: #000;border-radius: 50%;}
.go-back{width: 34px;height: 28px;display: inline-block;background: url('back.png') no-repeat;}
.food-content{padding: 18px;position: relative;}
.food-title{line-height: 14px;margin-bottom: 8px;font-size: 14px;font-weight: 700;color: rgb(7,17,27);}
.food-detail{margin-bottom: 8px;line-height: 10px;font-size: 0;}
.food-detail .sell-count,.rating{font-size: 10px;color: rgb(147,153,159);}
.food-detail .sell-count{margin-right: 12px;}
.food-content .price{font-weight: 700;line-height: 24px;}
.food-content .price .now{margin-right: 8px;font-size: 14px;color: rgb(240,20,20);}
.food-content .price .old{text-decoration: line-through;font-size: 10px;color: rgb(147,153,159);}
.carcontrol-wrapper{position: absolute;right: 18px;bottom: 18px;}
.buy{position: absolute;right: 18px;bottom: 18px;z-index: 10;height: 24px;line-height: 24px;padding: 0 12px;box-sizing: border-box;font-size: 10px;border-radius: 12px;color: #fff;background: rgb(0,160,220);}
.info{padding: 18px;}
.info-title{line-height: 14px;margin-bottom: 6px;font-size: 14px;color: rgb(7,17,27);}
.info-text{line-height: 24px;padding: 0 8px;font-size: 12px;color: rgb(77,85,93);}
.rating{padding-top: 18px;}
.rating-title{line-height: 14px;margin-left: 18px;font-size: 14px;color: rgb(7,17,27);}
.rating-wrapper{padding: 0 18px;}
.rating-item{position: relative;padding: 16px 0;border-bottom: 1px solid rgba(7,17,27,0.1);}
.user{position: absolute;right: 0;top: 16px;font-size: 0;line-height: 12px;}
.user-name{display: inline-block;vertical-align: top;font-size: 10px;color: rgb(147,153,159);margin-right: 6px;}
.user-avatar{border-radius: 50%;}
.user-time{margin-bottom: 6px;line-height: 12px;font-size: 10px;color: rgb(147,153,159);}
.user-text{line-height: 16px;font-size: 12px;color: rgb(7,17,27);vertical-align: top;}
.user_up{display: inline-block;width: 20px;height: 20px;background: url("user_up.png") no-repeat;background-size: 20px 20px;line-height: 16px;margin-right: 4px;vertical-align: top;}
.user_down{display: inline-block;width: 20px;height: 20px;background: url("user_down.png") no-repeat;background-size: 20px 20px;line-height: 16px;margin-right: 4px;vertical-align: top;}
.no-rating{padding: 16px 0;font-size: 12px;color: rgb(147,153,159);}
</style>
