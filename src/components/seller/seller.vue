<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="seller-overview">
				<h1 class="seller-title">{{seller.name}}</h1>
				<div class="desc">
					<star :size="48" :score="seller.foodScore"></star>
					<span class="seller-text">（{{seller.ratingCount}}）</span>
					<span class="seller-text">月售{{seller.sellCount}}单</span>
				</div>
				<ul class="seller-remark">
					<li class="seller-block">
						<h2>起送价</h2>
						<div class="seller-remark-content">
							<span class="seller-remark-stress">{{seller.minPrice}}<span class="unit">元</span></span>
						</div>
					</li>
					<li class="seller-block">
						<h2>商家配送费</h2>
						<div class="seller-remark-content">
							<span class="seller-remark-stress">{{seller.deliveryPrice}}<span class="unit">元</span></span>
						</div>
					</li>
					<li class="seller-block">
						<h2>平均配送时间</h2>
						<div class="seller-remark-content">
							<span class="seller-remark-stress">{{seller.deliveryTime}}<span class="unit">分钟</span></span>
						</div>
					</li>
				</ul>
				<div class="favorite">
					<span class="icon-favorite" :class="{'isFavorite':favorite}" @click="toggleFavorite()"></span>
					<span class="icon-text">{{favoriteText}}</span>
				</div>
			</div>
			<split></split>
			<div class="seller-bulletin">
				<h1 class="seller-title">公告与活动</h1>
				<div class="seller-content-wrapper">
					<p class="seller-content-wrapper-content">{{seller.bulletin}}</p>
				</div>
				<ul v-if="seller.supports" class="supports">
      		   	  <li class="support-item" v-for="(item,index) in seller.supports">
      		   	  	 <span class="icon" :id="classMap[seller.supports[index].type]"></span>
      		   	  	 <span class="text">{{seller.supports[index].description}}</span>
      		   	  </li>
      		   </ul>
			</div>
			<split></split>
			<div class="seller-pic">
				<h1 class="seller-title">商家实景</h1>
				<div class="seller-pic-wrapper" ref="picWrapper">
					<ul class="seller-pic-list" ref="picList">
						<li class="seller-pic-item" v-for="pic in seller.pics">
							<img :src="pic" width="120" height="90">
						</li>
					</ul>
				</div>
			</div>
			<split></split>
			<div class="seller-info">
				<h1 class="seller-title">商家信息</h1>
				<ul>
					<li v-for="info in seller.infos" class="info-item">{{info}}</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>	
import star from '../star/star.vue'
import split from '../split/split'
import BScroll from 'better-scroll'

export default{
	props:{
		seller:{
			type:Object
		}
	},
	data(){
		return{
			favorite:false
		}
	},
	computed:{
		favoriteText(){
			return this.favorite?'已收藏':'收藏';
		}
	},
	methods:{
		toggleFavorite(){
			this.favorite =!this.favorite;
		}
	},
	components:{
		star,
		split
	},
	created(){
       this.classMap = ['decrease','discount','special','invoice','guarantee'];
       this.$nextTick(() => {
       	this.scroll = new BScroll(this.$refs.seller,{
     		click:true
     	});
       }) 
     },
     mounted(){
     	if (this.seller.pics) {
     		let picWidth = 120;
     		let margin = 6;
     		let width = (picWidth + margin) * this.seller.pics.length;
     		this.$refs.picList.style.width = width + "px";
     		this.$nextTick(()=>{
     			this.picScroll = new BScroll(this.$refs.picWrapper,{
     				scrollX:true
     			});
     		});
     	}else{
     	}
     }
}
</script>

<style scoped>
.seller{position: absolute;top: 174px;bottom: 0;left: 0;width: 100%;overflow: hidden;}
.seller-overview{padding: 18px;position: relative;}
.seller-title{margin-bottom: 8px;line-height: 14px;color: rgb(7,17,27);font-size: 14px;}
.desc{padding-bottom: 18px;font-size: 0;}
.star{display: inline-block;margin-right: 8px;vertical-align: top;margin-left: -5px;}
.seller-text{margin-right: 12px;display: inline-block;vertical-align: top;font-size: 10px;line-height: 18px;}
.seller-remark{display: flex;padding-top: 18px;border-top: 1px solid rgba(7,17,27,0.1);}
.seller-block{flex: 1;text-align: center;border-right: 1px solid rgba(7,17,27,0.1);}
.seller-block:last-child{border:none;}
.seller-block h2{margin-bottom: 4px;line-height: 10px;font-size: 10px;color: rgb(147,153,159);}
.seller-remark-content{line-height: 24px;font-size: 10px;color: rgb(7,17,27);}
.seller-remark-stress{font-size: 24px;}
.unit{font-size: 10px;color: rgb(7,17,27);}
.seller-bulletin{padding: 18px 18px 0 18px;}
.seller-bulletin .seller-title{margin-bottom: 8px;line-height: 14px;color: rgb(7,17,27);font-size: 14px;}
.seller-content-wrapper{padding:0 12px 16px 12px;border-bottom: 1px solid rgba(7,17,27,0.1);}
.seller-content-wrapper-content{line-height: 24px;color: rgb(240,20,20);font-size: 12px;}

.supports .icon{display: inline-block;width: 12px;height: 12px;background-size:12px 12px;background-repeat: no-repeat;vertical-align: middle;}
.support-item{padding: 16px 6px 16px 12px;border-top: 1px solid rgba(7,17,27,0.1);}
 #decrease{background-image: url("decrease_4@2x.png");}
 #discount{background-image: url("discount_4@2x.png");}
 #special{background-image: url("special_4@2x.png");}
 #invoice{background-image: url("invoice_4@2x.png");}
 #guarantee{background-image: url("guarantee_4@2x.png");}
.supports .text{font-size: 12px;color: rgb(7,17,27);line-height: 16px;vertical-align: top;}

.seller-pic{padding: 18px;}
.seller-pic-wrapper{width: 100%;overflow: hidden;white-space: nowrap;}
.seller-pic-list{font-size: 0;}
.seller-pic-item{display: inline-block;margin-right: 6px;width: 120px;height: 90px;}
.seller-pic-item:last-child{margin-right: 0;}

.seller-info{padding: 18px;}
.info-item{padding: 16px 12px;line-height: 16px;border-bottom: 1px solid rgba(7,17,27,0.1);font-size: 12px;color: rgb(7,17,27);}
.info-item:last-child{border: none;}

.favorite{position: absolute;right: 18px;top: 18px;text-align: center;}
.icon-favorite{display: block;width: 20px;height: 20px;background: url('nosave.png') no-repeat;background-size: 20px 20px;margin: 0 auto;}
.isFavorite{background: url('save.png') no-repeat;background-size: 20px 20px;margin-right: 2px;}
.icon-text{display: block; font-size: 10px;color: rgb(77,85,93);line-height: 10px;margin-top: 4px;}
</style>
