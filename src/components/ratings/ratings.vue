<template>
   <div class="ratings" ref="ratings">
   	  <div class="ratings-content">
   	  	<div class="overview">
   	  		<div class="overview-left">
   	  			<h1 class="scroe">{{seller.score}}</h1>
   	  			<div class="ratings-title">综合评价</div>
   	  			<div class="rank">高于周围商家{{seller.rankRate}}</div>
   	  		</div>
   	  		<div class="overview-right">
   	  			<div class="score-wrapper">
   	  				<span class="score-title">服务态度</span>
   	  				<star :size="48" :score="seller.serviceScore"></star>
   	  				<span class="score">{{seller.serviceScore}}</span>
   	  			</div>
   	  			<div class="score-wrapper">
   	  				<span class="score-title">商品评分</span>
   	  				<star :size="48" :score="seller.foodScore"></star>
   	  				<span class="score">{{seller.foodScore}}</span>
   	  			</div>
   	  			<div class="deliver-wrapper">
   	  				<span class="deliver-title">送达时间</span>
   	  				<span class="deliver">{{seller.deliveryTime}}分钟</span>
   	  			</div>
   	  		</div>
   	  	</div>
   	  	<split></split>
   	  	<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
   	  	<div class="ratings-wrapper">
   	  		<ul>
   	  			<li v-for="rating in ratings" class="ratings-item">
   	  				<div class="ratings-avatar">
   	  					<img :src="rating.avatar" width="28" height="28" class="rating-user">
   	  				</div>
   	  				<div class="ratings-content">
   	  					<h1 class="ratings-name">{{rating.username}}</h1>
   	  					<div class="star-wrapper">
   	  					<star :size="48" :score="rating.score"></star>
   	  						<span class="ratings-deliver" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
   	  						<span :class="{'rating_up':rating.rateType===0,'rating_down':rating.rateType===1}"></span>
   	  					</div>
   	  					<p class="ratings-text">{{rating.text}}</p>
   	  				</div>
   	  			</li>
   	  		</ul>
   	  	</div>
   	  </div>
   </div>
</template>

<script >
import star from '../star/star.vue' 
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect' 
import BScroll from 'better-scroll'

const ALL = 2;

export default { 
    props:{
        seller:{
           type:Object
        }
    },
    components:{
     	star,
     	split,
     	ratingselect
     },
     data(){
     	return {
     		ratings:[],
			selectType:ALL,
			onlyContent:true,
     	}
     },
     created(){
     	this.$http.get('/api/ratings').then((response)=>{
           this.ratings = response.body.data;
           this.$nextTick(()=>{
           	  this.scroll = new BScroll(this.$refs.ratings,{
       	 		 click:true
       	 	});
           });
           	
     	});
     }
};

</script>

<style>
@media screen and (max-width: 320px){
	.overview-left{flex: 0 0 120px;	width: 120px;}
	.overview-right{padding: 6px 0 6px 6px;}
}
@media screen and (min-width: 370px){
	.overview-left{flex: 0 0 137px;	width: 137px;}
	.overview-right{padding: 6px 0 6px 24px;}
}
.ratings{position: absolute;top: 174px;bottom: 0;width: 100%;left: 0;overflow: hidden;}
.overview{display: flex;padding: 18px 0;}
.overview-left{border-right: 1px solid rgba(7,17,27,0.1);text-align: center;}
.scroe{line-height: 28px;color: rgb(255,153,0);font-size: 24px;margin-bottom: 6px;}
.ratings-title{line-height: 12px;font-size: 12px;color: rgb(7,17,27);margin-bottom: 6px;}
.rank{font-size: 10px;line-height: 10px;color: rgb(147,153,159);padding-bottom: 6px;}
.overview-right{flex: 1;}
.score-wrapper{margin-bottom: 8px;line-height: 18px;font-size: 0;}
.score-title,.deliver-title{font-size: 12px;color: rgb(7,17,27);display: inline-block;vertical-align: top;}
.star{display: inline-block;margin: 0 12px;vertical-align: top;}
.score{display: inline-block;vertical-align: top;font-size: 12px;}
.deliver-wrapper{font-size: 0;}
.deliver{font-size: 12px;color: rgb(147,153,159);margin-left: 12px;vertical-align: top;}
.ratings-wrapper{padding:0 18px;}
.ratings-item{display: flex;padding: 18px 0;border-bottom: 1px solid rgba(7,17,27,0.1);}
.ratings-item .avatar>img{flex: 0 0 28px;width: 28px;border-radius: 50%;margin-right: 12px;}
.rating-user{display: inline-block;border-radius: 50%;}
.ratings-content{flex: 1;position: relative;}
.ratings-name{margin-bottom: 4px;line-height: 12px;font-size: 10px;color: rgb(7,17,27);margin-left: 12px;}
.star-wrapper{margin-bottom: 6px;font-size: 0;}
.star{display: inline-block;margin-right: 6px;vertical-align: top;}
.ratings-deliver{font-size: 12px;font-weight: 200;color: rgb(147,153,159);line-height: 24px;vertical-align: top;}
.rating_up{display: inline-block;width: 20px;height: 20px;background: url("user_up.png") no-repeat;background-size: 20px 20px;line-height: 16px;margin-right: 4px;vertical-align: top;position: absolute;right: 25px;top: 18px;}
.rating_down{display: inline-block;width: 20px;height: 20px;background: url("user_down.png") no-repeat;background-size: 20px 20px;line-height: 16px;margin-right: 4px;vertical-align: top;position: absolute;right: 25px;top: 18px;}
.ratings-text{font-size: 12px;color: rgb(7,17,27);line-height: 18px;}
</style>
