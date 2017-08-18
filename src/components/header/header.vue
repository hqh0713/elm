<template>
   <div class="header">
      <div class="content-wrapper">
      	 <div class="avatar">
      	 	<img height="64" width="64" :src="seller.avatar">
      	 </div>
      	 <div class="content">
      	 	<div class="title">
      	 		<span class="brand"></span>
      	 		<span class="name">{{seller.name}}</span>
      	 	</div>
      	 	<div class="description">
      	 		{{seller.description}}/{{seller.deliveryTime}}分钟送达
      	 	</div>
      	 	<div v-if="seller.supports" class="support">
      	 		<span class="icon" :class="classMap[seller.supports[0].type]"></span>
      	 		<span class="text">{{seller.supports[0].description}}</span>
      	 	</div>
      	 </div>
      	 <div v-if="seller.supports" class="support-count" @click="detailShow=true">
      	 	<span class="count">{{seller.supports.length}}</span>
      	 	<i></i>
      	 </div>
      </div>
      <div class="bulletin-wrapper" @click="detailShow=true">
      	<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      	<i></i>
      </div>
      <div class="background">
      	<img :src="seller.avatar" width="100%" height="100%">
      </div>
      <!-- 浮层 -->
      <transition name="fade">
      <div class="detail" v-show="detailShow">
      	<div class="detail-wrapper clearfix">
      		<div class="detail-main">
      		   <h1 class="name">{{seller.name}}</h1>
      		   <div class="starWrapper"> <!-- 星星包裹层 -->
      		       <star :size="48" :score="seller.score"></star> 
      		   </div>
      		   <div class="detail-title"> <!-- 优惠信息层 -->
      		   	   <div class="line"></div>
      		   	   <div class="text">优惠信息</div>
      		   	   <div class="line"></div>
      		   </div> 
      		   <ul v-if="seller.supports" class="supports">
      		   	  <li class="support-item" v-for="(item,index) in seller.supports">
      		   	  	 <span class="icon" :id="classMap[seller.supports[index].type]"></span>
      		   	  	 <span class="text">{{seller.supports[index].description}}</span>
      		   	  </li>
      		   </ul> 
      		   <div class="detail-title"> <!-- 商家公告 -->
      		   	   <div class="line"></div>
      		   	   <div class="text">商家公告</div>
      		   	   <div class="line"></div>
      		   </div> 
      		   <div class="bulletin">
      		   	   <p class="content">{{seller.bulletin}}</p>
      		   </div>
      		</div>
      	</div>
      	<div class="detail-close" @click="detailShow=false">
      		<i></i>
      	</div>
      </div>
      </transition>
   </div>
</template>

<script >	
   import star from '../star/star.vue'  

   export default { 
     props: {
       seller: {
         type: Object
       }
     },
     data() {
       return {
         detailShow: false
       };
     },
     methods:{

     },
     created() {
       this.classMap = ['decrease','discount','special','invoice','guarantee'];
     },
     components:{
     	star
     }
   };
</script>

<style scoped>
@media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3){
	.brand{background-image: url("brand@3x.png"); }
	.bulletin-title{background-image: url("bulletin@3x.png");}
	#decrease{background-image: url("decrease_2@3x.png");}
	#discount{background-image: url("discount_2@3x.png");}
	#special{background-image: url("special_2@3x.png");}
	#invoice{background-image: url("invoice_2@3x.png");}
	#guarantee{background-image: url("guarantee_2@3x.png");}
}
@media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2){
	.brand{background-image: url("brand@2x.png"); }
	.bulletin-title{background-image: url("bulletin@2x.png");}
	#decrease{background-image: url("decrease_2@2x.png");}
	#discount{background-image: url("discount_2@2x.png");}
	#special{background-image: url("special_2@2x.png");}
	#invoice{background-image: url("invoice_2@2x.png");}
	#guarantee{background-image: url("guarantee_2@2x.png");}
}
.header{color:#fff;position: relative;background:rgba(7,17,27,0.5); overflow: hidden;}
.content-wrapper{padding: 24px 12px 18px 24px;font-size: 0;position:relative; }
.avatar{display: inline-block;vertical-align: top;}
.avatar img{border-radius: 2px;}
.content{display: inline-block;font-size: 14px;margin-left: 16px;}
.title{margin: 2px 0 8px 0;}
.title .brand{display: inline-block; width: 30px;height: 18px;background-size:30px 18px;vertical-align: top;}
.title .name{margin-left: 6px;font-size: 16px;line-height: 18px;font-weight: bold;}
.description{margin-bottom: 10px;line-height: 12px;font-size:12px;}
.support .icon{display: inline-block;width: 12px;height: 12px;background-size:12px 12px;background-repeat: no-repeat; }
.decrease{background-image: url("decrease_1@2x.png");vertical-align: top;}
.text{font-size: 12px;line-height: 10px;}
.support-count{position: absolute;right: 12px;bottom: 18px;padding: 0 8px;height: 24px;line-height: 24px;border-radius: 14px;background:rgba(0,0,0,0.2);text-align: center;}
.count{font-size: 10px;}
i{background: url("jt.png") no-repeat;display: inline-block;width: 8px;height:8px;margin-left: 4px;line-height: 24px;}
.bulletin-wrapper{height: 28px;line-height: 28px;padding: 0 22px 0 12px;white-space: nowrap;overflow: hidden;text-overflow: ellipsis;position: relative;background:rgba(7,17,27,0.2); }
.bulletin-title{display: inline-block;width: 22px;height: 12px;background-size: 22px 12px;vertical-align: top;margin-top: 8px;}
.bulletin-text{font-size: 10px;font-weight: 200;margin: 0 4px;vertical-align: top;}
.bulletin-wrapper i{background: url("jt.png") no-repeat;display: inline-block;width: 8px;height:8px;margin-left: 4px;line-height: 24px;position: absolute;right: 12px;top:10px;}
.background{position: absolute;top: 0;left: 0;width: 100%;height: 100%;z-index: -1;filter: blur(10px);}
.detail{position: fixed;z-index: 10;width: 100%;height: 100%;overflow: auto;top: 0;left: 0;background:rgba(7,17,27,0.8);transition: all 0.5s;backdrop-filter:blur(10px);}
.fade-enter-active{opacity: 1;}
.fade-enter,.fade-leave-active{opacity: 0;background:rgba(7,17,27,0); }
.detail-wrapper{min-height: 100%;width: 100%;}
.detail-main{margin-top: 64px;padding-bottom: 64px;}
.detail-main>.name{line-height: 16px;text-align: center;font-size: 16px;font-weight: 700;}
.detail-close{position: relative;width: 32px;height: 32px;margin: -64px auto 0 auto;clear: both;font-size: 32px;}
.detail-close i{background: url("close.png") ;display: block;width: 32px;height: 32px;background-size: 32px 32px;}
.starWrapper{margin-top: 18px;padding: 2px 0;text-align: center;}
.detail-title{display: flex;width: 80%;margin: 28px auto 24px auto;}
.detail-title .line{flex: 1;position: relative;top: -6px;border-bottom: 1px solid rgba(255,255,255,0.2);}
.detail-title .text{padding: 0 12px;font-size: 14px;font-weight: 700;}
.supports{width: 80%;margin: 0 auto;}
.supports .support-item{padding: 0 12px;margin-bottom: 12px;font-size: 0;}
.supports .support-item:last-child{margin-bottom: 0;}
.support-item .icon{display: inline-block;width: 16px;height: 16px;vertical-align: top;margin-right: 6px;background-size: 16px 16px;background-repeat: no-repeat;}
.support-item .text{font-size: 12px;font-weight: 200;line-height: 16px;color: rgb(255,255,255);}
.bulletin{width: 80%;margin: 0 auto;}
.bulletin .content{padding: 0 12px;line-height: 24px;font-size: 12px;}
</style>
