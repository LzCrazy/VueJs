<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="arrow_left"></i>
          </div>
        </div>
        <div class="contents">
          <h1 class="title">{{food.name}}</h1>
          <div class="details">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol @add="addFood" :food="food"></cartcontrol>
          </div>
          <transition name="fade">
          	<div @click.stop.prevent="addFirst" class="buy" v-show="!food.count||food.count===0">
          		加入购物车
          	</div>
          </transition>
        </div>

        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect @toggle="toggleContent" @select="selectRating" :selectType="selectType":onlyContent="onlyContent" :desc="desc":ratings="food.ratings"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings"
                  class="rating-item">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" width="12" height="12" :src="rating.avatar">
                </div>
                <div class="time">{{rating.rateTime}}</div>
                <p class="text">
                  <span>0</span>
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



<!-- <template>
	<transition name="move">
		<div v-show="showFlag" class="food" ref="food">
			<div class="food-content">
				<div class="image-header">
					<img :src="food.image">
					<div class="back" @click="hide">
						<i class="arrow_left"></i>
					</div>
				</div>
				<div class="content">
					<h1 class="title">{{food.name}}</h1>
					<div class="detailss">
						<span class="sell-count">月售{{food.sellCount}}份</span>
						<span class="rating">好评率{{food.rating}}%</span>
					</div>
					<div class="price">
						<span class="now">¥{{food.price}}</span>
						<span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
					</div>
				</div>
				<div class="cartcontrol-wrapper">
					<cartcontrol  :food="food"></cartcontrol>
				</div>
				<transition name="fade">
					<div @click.stop.prevent="addFirst" class="buy" v-show="!food.count||food.count===0">
						加入购物车
					</div>
				</transition>
			</div>
			<split v-show="food.info"></split>
			<div class="info" v-show="food.info">
				<h1 class="title">商品信息</h1>
				<p class="text">{{food.info}}</p>
			</div>
			<div class="rating">
				<h1 class="title">商品评价</h1>
				<ratingselect :selectType="selectType":onlyContent="onlyContent" :desc="desc":ratings="food.ratings"></ratingselect>
				<div class="rating-wrapper">
					<ul v-show="food.ratings && food.ratings.length">
						<li class="rating-item" v-for="rating in food.ratings">
							<div class="user">
								<span class="name">{{rating.username}}</span>
								<img :src="rating.avatar" width="12" height="12" class="avatar">
							</div>
							<div class="time">{{rating.rateTime}}</div>
							<p class="text">
								<span></span>
								{{rating.text}}
							</p>
						</li>
					</ul>
					<div class="no-rating" v-show="!food.ratings || !food.ratings">暂无评价</div>
				</div>
			</div>
		</div>
	</transition>
</template> -->

<script>
import BScroll from 'better-scroll';
import cartcontrol from '@/components/cartcontrol/cartcontrol';
import split from '@/components/split/split';
import ratingselect from '@/components/ratingselect/ratingselect';
import Vue from 'vue';
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
			onlyContent:true,
			desc:{
				all:'全部',
				positive:'推荐',
				negative:'吐槽'
			}
		}
	},
    methods: {
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food,{
              click:true
            });
          } else {
            this.scroll.refresh();
          }
         console.log(this.$refs.food);
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('add', event.target);
        Vue.set(this.food, 'count', 1);
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
      // 添加食物造成冒泡
      addFood(target) {
      	console.log(target+"target");
        this.$emit('add', target);
      },
      selectRating(type){
      	this.selectType = type;
      	this.$nextTick(()=>{
      		this.scroll.refresh();
      	})
      },
      toggleContent() {
      	console.log("tocontent");
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
	components:{
		cartcontrol,
		split,
		ratingselect
	}
};
</script>

<style>
.food{
	position: fixed;
	left: 0;
	top: 0;
	bottom: 48px;
	z-index: 30;
	width: 100%;
	background:#fff;
	transform: translate3d(0,0,0);
}
.move-enter-active,.move-leave-active{
	transition:all 0.2s linear;
}
.move-enter,.move-leave-active{
	transform:translate3d(100%,0,0);
}   
.food .image-header{
	position: relative;
	width: 100%;
	height: 0;
	padding-top: 100%;
}  
.image-header img{
	position: absolute;;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.image-header .back{
	position: absolute;
	top: 10px;
	left: 0;
}
.image-header .back .arrow_left{
  display: inline-block;
  width: 12px;  
  height: 12px;  
  left: 15px;
  position: absolute;  
  border-left: 1px solid #999;  
  border-bottom: 1px solid #999;  
  -webkit-transform: translate(0,50%) rotate(-315deg);  
  transform: translate(0,50%) rotate(-315deg);  
}
.food-content .contents{
	position: relative;
	padding: 18px;
}
 
.contents .title{
	line-height: 14px;
	margin-bottom: 8px;
	font-size: 14px;
	font-weight: 700;
	color: rgb(7,17,27);
}
.contents .details{
	margin-bottom: 18px;
	line-height: 10px;
	height: 10px;
	font-size: 0;
}
.contents .details .sell-count,.contents .details .rating{
	font-size: 10px;
	color: rgb(147,153,159);
}
.contents .details .sell-count{
	margin-right: 12px;
}
.contents .price{
	font-weight: 700;
	line-height: 14px;
}
.contents .price .now{
	margin-right: 8px;
	font-size: 14px;
	color: rgb(240,20,20);
}
.contents .price .old{
	text-decoration: line-through;
	font-size: 10px;
	color: rgb(147,153,159);
}
.food-content .cartcontrol-wrapper{
	position: absolute;
	right: 12px;
	bottom: 12px;
}
.contents .buy{
	position: absolute;
	right: 0;
	bottom: 18px;
	z-index: 10;
	height: 24px;
	line-height: 24px;
	padding: 0 12px;
	box-sizing: border-box;
	border-radius: 12px;
	font-size: 10px;
	color: #fff;
	background: rgb(0,160,220);
}
.fade-enter-active,.fade-leave-active{
	transition: all 0.5s;
}
.fade-enter,.fade-leave-active{
	opacity: 0;
	z-index: -1;
}
.food .info{
	padding: 18px;
}
.info .title{
	line-height: 14px;
	margin-bottom: 6px;
	font-size: 14px;
	color: rgb(7,17,27);
}
.info .text{
	line-height: 24px;
	padding:0 8px;
	font-size: 12px;
	color: rgb(77,85,93);
}
.food .rating{
	padding-top: 18px;

}
.rating .title{
	line-height: 24px;
	margin-left: 18px;
	font-size: 14px;
	color:rgb(7,17,27);

}
.rating .rating-wrapper{
	padding: 0 18px;
}
.rating-wrapper .rating-item{
	position: relative;
	padding: 16px 0;
	border-bottom: 1px solid rgba(7,17,27,0.1);
}
.rating-item{
	position: absolute;
	right: 0;
	top: 16px;
	line-height: 12px;
	border-bottom: 1px solid rgba(7,17,27,0.1);
}
.rating-item .user{
	position: absolute;
	right: 0;
	top: 16px;
	line-height: 12px;
	font-size: 0;
}
.user .name{
	display: inline-block;
	margin-right: 6px;
	font-size: 10px;
	color: rgb(147,153,159);
	vertical-align: top;
}
.user .avatar{
	border-radius: 50%;
}
.rating-item .time{
	margin-bottom: 6px;
	font-size: 10px;
	line-height: 12px;
	color: rgb(147, 153, 159);
}
.rating-item .text{
	line-height: 16px;
	font-size: 12px;
	color: rgb(7,17,27);
}
.no-rating{
	padding:16px 0;
	font-size: 12px;
	color: rgb(147,153,159);
}

</style>