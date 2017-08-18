<template>
	<div class="shopCar">
		<div class="shopCar-content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highLight':totalNum>0}">
						<span  class="icon-shopcar"></span>
					</div>
					<div class="num">{{totalNum}}</div>
				</div>
				<div class="price">￥{{totalPrice}}</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}</div>
			</div>
			 <div class="content-right" @click.stop.prevent="pay">
				<div class="pay" :class="payClass">
					{{payDesc}}
				</div>
			</div>
		</div>
          <transition name="fold">
             <div class="shopcar-list" v-show="listShow">
               <div class="list-header">
                    <h1 class="fold-title">购物车</h1>
                    <span class="empty" @click="empty">清空</span>
               </div>
               <div class="list-content" ref="listContent">
                    <ul>
                         <li class="food" v-for="food in selectFoods">
                              <span class="name">{{food.name}}</span>
                              <div class="price">
                                   <span>￥{{food.price*food.count}}</span>
                              </div>
                              <div class="carcontrol-wrapper">
                                   <car-control :food="food"></car-control>
                              </div>
                         </li>
                    </ul>
               </div>
             </div>
          </transition>
          <transition name="fade">
               <div class="list-mask" v-show="listShow" @click="hiddenList"></div>
          </transition>   
	</div>
</template>

<script>
import BScroll from 'better-scroll'
import carControl from '../carcontrol/carControl'

export default {
	props: {
       selectFoods: {
         type: Array,
         default(){
            return [];
         }
       },
       minPrice:{
          type:Number,
          default:0
       },
       deliveryPrice:{
          type:Number,
          default:0
       },
       seller:{
          type:Object
       }
     },
     data(){
     	return {
               fold:true
               }
     },
     methods:{
          toggleList(){
               if (!this.totalNum) {
                    return;
               }
               this.fold=!this.fold;
          },
          empty(){
               this.selectFoods.forEach((food)=>{
                    food.count = 0;
               });
          },
          hiddenList(){
               this.fold = true;
          },
          pay(){
               if (this.totalPrice<this.minPrice) {
                    return;
               }
               window.alert('支付'+this.totalPrice+'元');
          }
     },
     computed:{
          totalPrice(){
               let totalMoney = 0
               this.selectFoods.forEach((food)=>{
                    totalMoney+=food.price * food.count;
               });
               return totalMoney;
          },
          totalNum(){
               let totalNum=0;
               this.selectFoods.forEach((food)=>{
                    totalNum+=food.count;
               });
               return totalNum;
          },
          payDesc(){
               if (this.totalPrice===0) {
                    return '￥'+this.seller.minPrice+"元起送";
               }else if(this.totalPrice > 0 && this.totalPrice < this.seller.minPrice){
                    return "还差"+(this.seller.minPrice-this.totalPrice)+"元起送";
               }else{
                    return "去结算";
               }
          },
          payClass(){
               if (this.totalPrice>=this.seller.minPrice) {
                    return 'enough';
               }else{
                    return '';
               }
          },
          listShow(){
               if (!this.totalNum) {
                    this.fold = true;
                    return false;
               }
               let show = !this.fold;
               if (show) {
                    this.$nextTick(()=>{
                         if (!this.scroll) {
                              this.scroll = new BScroll(this.$refs.listContent,{
                              click:true
                            });
                         }else{
                              this.scroll.refresh();
                         }
                    });
               }
               return show;
          }
     },
     components:{
          'car-control':carControl
     }
};
</script>

<style>
.shopCar{position: fixed;left: 0;bottom: 0;z-index: 20;width: 100%;height: 48px;}
.shopCar .shopCar-content{display: flex;background: #141d27;font-size: 0;}
.shopCar .shopCar-content .content-left{flex: 1;}
.shopCar .shopCar-content .content-left .logo-wrapper{display: inline-block;position: relative;top: -10px;margin: 0 12px;padding: 6px;width: 56px;height: 56px;box-sizing: border-box;vertical-align: top;border-radius: 50%;background-color: #141d27;}
.shopCar .shopCar-content .content-left .logo{width: 100%;height: 100%;border-radius: 50%;text-align: center;background-color: #2b343c;}
/*购物车高亮*/
.shopCar .shopCar-content .content-left .logo.highLight{background-color: rgb(0,160,220);}

.icon-shopcar{background: url('logo-car.png') no-repeat;display: inline-block;width: 42px;height: 42px;background-size: 42px 42px;line-height: 44px;font-size: 24px;color: #80858a;}
.shopCar-content .content-left .price,.desc{display: inline-block;vertical-align: top;margin-top: 12px;line-height: 24px;box-sizing: border-box;padding-left: 5px; border-right:1px solid rgba(255,255,255,0.1);font-size: 16px;font-weight: 700;color: #919396;}
.shopCar-content .content-left .desc{border-right:none;font-size: 10px;font-weight: 200;}
.shopCar-content .content-right{flex: 0 0 105px;width: 105px;}
.content-right .pay{height: 48px;line-height: 48px;text-align: center;font-size: 12px;font-weight: 700; color: rgba(255,255,255,0.4);}
/*结算*/
.shopCar-content .content-right .pay.enough{background-color: #00b43c;color: #fff;}
.logo-wrapper .num{position: absolute;top: 0;right: 0;width: 24px;height: 16px;line-height: 16px;text-align: center;border-radius: 16px;font-size: 9px;font-weight: 700;color: #fff;background-color: rgb(240,20,20);box-shadow: 0 4px 8px 0 rgba(0,0,0,.0.4);}
.shopcar-list{position: fixed;left: 0;bottom: 48px; z-index: 20;width: 100%;}
.shopcar-list .list-header{height: 40px;line-height: 40px;padding: 0 18px;background-color: #f3f5f7;border-bottom: 2px solid rgba(7,17,27,0.1);}
.shopcar-list .list-header .fold-title{float: left;font-size: 17px;color: rgb(7,17,27);}
.shopcar-list .list-header .empty{float: right;font-size: 12px;color: rgb(0,160,220);}
.fold-enter-active{transition: all 0.5s linear;opacity: 1;}
.fold-enter,.fold-leave-active{opacity: 0;}
.list-content{padding: 0 18px;max-height: 217px;background-color: #fff;overflow: hidden;}
.list-content .food{position: relative;padding: 12px 0;box-sizing: border-box;border-bottom: 1px solid rgba(7,17,27,0.1);}
.list-content .food .name{line-height: 24px;font-size: 14px;color: rgb(7,17,27);}
.list-content .food .price{position: absolute;right: 90px;bottom: 12px;line-height: 24px;font-size: 14px;font-weight: 700;color: rgb(240,20,20);}
.list-content .food .carcontrol-wrapper{position: absolute;right: 0;bottom: 6px;}
.list-mask{position: fixed;top: 0;left: 0;width: 100%;height: 100%;z-index: 15;backdrop-filter:blur
     (10px);transition: all 0.5s;background-color: rgba(7,17,27,0.6);}
.fade-enter-active{background-color: rgba(7,17,27,0.6);}
.fade-enter,.fade-leave-active{background-color: rgba(7,17,27,0.6);}
</style>
