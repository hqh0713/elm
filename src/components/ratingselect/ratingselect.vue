<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span class="block positive" @click="select(2,$event)" :class="{'active':selectType===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
			<span class="block positive"  @click="select(0,$event)" :class="{'active':selectType===0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
			<span class="block negative"  @click="select(1,$event)" :class="{'active':selectType===1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
		</div>
		<div class="switch"  @click="toggleContent">
			 <span class="gou" :class="{'on':onlyContent}"></span>
			 <span class="switch-text">只看有内容评价</span>
		</div>
	</div>
</template>

<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;

export default{
	props:{
		ratings:{
			type:Array,
			default(){
				return [];
			}
		},
		selectType:{
			type:Number,
			default:ALL
		},
		onlyContent:{
			type:Boolean,
			default:false
		},
		desc:{
			type:Object,
			default(){
				return {
					all:'全部',
					positive:'满意',
					negative:'不满意'
				}
			}
		}
	},
	methods:{
		select(type,event){
			if (!event._constructed) {
				return;
			}
			this.selectType = type;
			this.$emit('ratingtype.select',type); //通过$emit将selectType值的变化通知父组件
		},
		toggleContent(event){
			if (!event._constructed) {
				return;
			}
			this.onlyContent = !this.onlyContent;
			this.$emit('content.toggle',this.onlyContent);
			console.log(this.onlyContent);
		}
	},
	computed:{
		positives(){
			return this.ratings.filter((rating)=>{
				return rating.rateType === POSITIVE;
			});
		},
		negatives(){
			return this.ratings.filter((rating)=>{
				return rating.rateType === NEGATIVE;
			});
		}
	}
};	
</script>

<style>
.ratingselect{border-bottom: 1px solid rgb(7,17,27,0.2);margin-top: 18px;}
.rating-type{position: 18px 0;margin: 0 18px;font-size: 0;}	
.block{display: inline-block;padding: 8px 12px;margin-right: 8px;border-radius: 1px;color: rgb(77,85,93);font-size: 12px;}
.block.active{color: #fff}
.block.positive{background: rgba(0,160,220,0.2);}
.block.positive.active{background: rgb(0,160,220);}
.block.negative{background: rgba(77,85,93,0.2);}
.block.negative.active{background: rgb(77,85,93);}
.block .count{font-size: 8px;margin-left: 2px;line-height: 16px;}
.switch{padding: 12px 18px;line-height: 24px;border-bottom: 1px solid rgba(7,17,27,0.1);color: rgb(147,153,159);font-size: 0;}
.switch .gou{width: 24px;height: 24px;background: url('gou.png') no-repeat;background-size: 24px 24px;display: inline-block;line-height: 24px;margin-right: 4px;vertical-align: top;}
.switch .gou.on{background: url('gou-act.png') no-repeat;background-size: 24px 24px;}
.switch-text{font-size: 12px;vertical-align: top;}
</style>
