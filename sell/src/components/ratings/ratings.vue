<template>
	<div class="ratings" ref="ratings">
		<div class="ratings-content">
			<div class="overview">
			  <div class="overview-left">
			    <h1 class="score">{{seller.score}}</h1>
			    <div class="title">综合评分</div>
			    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
			  </div>
			  <div class="overview-right">
			    <div class="score-wrapper">
			      <span class="title">服务态度</span>
			    <!--   <star :size="36" :score="seller.serviceScore"></star> -->
			      <span class="score">{{seller.serviceScore}}</span>
			    </div>
			    <div class="score-wrapper">
			      <span class="title">商品评分</span>
			    <!--   <star :size="36" :score="seller.foodScore"></star> -->
			      <span class="score">{{seller.foodScore}}</span>
			    </div>
			    <div class="delivery-wrapper">
			      <span class="title">送达时间</span>
			      <span class="delivery">{{seller.deliveryTime}}分钟</span>
			    </div>
			  </div>
			</div>
			<split></split>
			<ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingselect>
			<div class="rating-wrapper">
				<ul>
					<li class="rating-item" v-for="rating in ratings" v-show="needShow(rating.rateType,rating.text)">
						<div class="avatar">
							<img :src="rating.avatar" height="28" width="28">
						</div>
						<div class="content">
							<h1 class="name">{{rating.username}}</h1>
							<div class="star-wrapper">
								<!-- <star :size="24" :score="rating.score"></star> -->
								<span class="delivery">
									{{rating.deliveryTime}}分钟
								</span>
							</div>
							<p class="text">
								{{rating.text}}
							</p>
							<div class="recommend">
								<span class="thumb_up"></span>
								<span class="item" v-for="item in rating.recommend">
									{{item}}
								</span>
							</div>
							<div class="time">
								{{rating.rateTime}}
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import ratingselect from '@/components/ratingselect/ratingselect';
import split from '@/components/split/split';
import star from '@/components/star/star';
import BScroll from 'better-scroll';
const ALL =2;
const ERR_OK = 0;
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return {
				ratings:[],
				selectType:ALL,
				onlyContent:true
			}
		},
		created(){
			const url = 'api/ratings';
			this.$http.get(url).then((response)=>{
				response = response.body;
				if(response.errno === ERR_OK){
					this.ratings =response.data;
					this.$nextTick(()=>{
						this.scroll = new BScroll(this.$refs.ratings,{
							click:true
						})
					})
				}
			})
		},
		methods:{
			selectRating(type){
				this.selectType = type;
				this.$nextTick(()=>{
					this.scroll.refresh();
				})
			},
			needShow(type,text){
				if(this.onlyContent && !text){
					return false;
				}
				if(this.selectType === ALL){
					return true;
				}else{
					return type === this.selectType;
				}
			},
			toggleContent(){
				this.onlyContent = !this.onlyContent;
				this.$nextTick(()=>{
					this.scroll.refresh();
				})
			},
		},
		components:{
			ratingselect,
			star,
			split
		}
	}
</script>

<style scoped>
.ratings{
	/*display: flex;*/
	position: absolute;
	top: 176px;
	bottom: 0;
	left: 0;
	width: 100%;
	overflow: hidden;
}
.ratings .overview{
	display: flex;
	padding:18px;
}
.overview .overview-left{
	flex: 0 0 137px;
	padding: 6px 0;
	width: 137px;
	border-right: 1px solid rgba(7,17,27,0.1);
	text-align: center;
	@media only screen and (max-width: 320px);
	flex: 0 0 120px;
	width: 120px;
}
.overview-left .score{
	margin-bottom: 6px;
	line-height: 28px;
	font-size: 24px;
	color: rgb(255,153,0);
}
.overview-left .title{
	margin-bottom: 6px;
	line-height: 12px;	
	font-size: 12px;
	color: rgb(7,17,27);
}
.overview-left .rank{
	line-height: 10px;
	font-size: 10px;
	color: rgb(147,153,159);
}
.overview .overview-right{
	flex: 1;
	padding: 6px 0 6px 24px;
	@media only screen and (max-width:320px);
	padding-left: 6px;
}
.overview-right .score-wrapper{
	margin-bottom: 8px;
	font-size: 0;
}
.score-wrapper .title{
	display: inline-block;
	line-height: 18px;
	vertical-align: top;
	font-size: 12px;
	color: rgb(7,17,27);
}
/*.score-wrapper .star{
	display: inline-block;
	margin:0 12px;
	vertical-align: top;
}*/
.score-wrapper .score{
	display: inline-block;
	line-height: 18px;
	vertical-align: top;
	font-size: 12px;
	color: rgb(255,153,0);
}
.overview .delivery-wrapper{
	font-size: 0;
}
.delivery-wrapper .title{
	line-height: 18px;
	font-size: 12px;
	color: rgb(7,17,27);
}
.delivery-wrapper .delivery{
	margin-left: 12px;
	font-size: 12px;
	color: rgb(147,153,159);
}
.ratings .rating-wrapper{
	padding:0 18px;
}
.rating-wrapper .rating-item{
	display: flex;
	padding:18px 0;
	border-bottom: 1px solid rgba(7,17,27,0.1);
}
.rating-item .avatar{
	flex: 0 0 28px;
	width: 28px;
	margin-right: 12px;
}
.avatar img{
	border-radius: 50%;
}
.rating-item .content{
	position: relative;
	flex: 1;
}
.content .name{
	margin-bottom: 4px;
	line-height: 12px;
	font-size: 10px;
	color: rgb(7,17,27);
}
.content .star-wrapper{
	margin-bottom: 6px;
	font-size: 0;
}
/*.star-wrapper .star{
	display: inline-block;
	margin-right: 6px;
	vertical-align: top;
}*/
.star-wrapper .delivery{
	display: inline-block;
	vertical-align: top;
	line-height: 12px;
	font-size: 10px;
	color: rgb(147,153,159);
}
.content .text{
	margin-bottom: 8px;
	line-height: 18px;
	color: rgb(7,17,27);
	font-size: 12px;
}
.content .recommend{
	line-height: 16px;
	font-size: 0;
}
.recommend .thumb_up,.recommend .item{
	display: inline-block;
	margin:0 8px 4px 0;
	font-size: 9px;
}
.recommend .thumb_up{
	color: rgb(0,160,220);
}
.recommend .item{
	padding: 0 6px;
	border:1px solid rgba(7,17,27,0.1);
	border-radius: 1px;
	color: rgb(147,153,159);
	background: #fff;
}
.content .time{
	position: absolute;
	top: 0;
	right: 0;
	line-height: 12px;
	font-size: 10px;
	color: rgb(147,153,159);
}


</style>