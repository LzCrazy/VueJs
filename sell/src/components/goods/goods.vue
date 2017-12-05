<template>
  <div>
    <div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
		    <ul>
		      <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}"
		          @click="selectMenu(index,$event)" ref="menuList">
		      <span class="text border-1px">
		        <span v-show="item.type>0" :class="classMap[item.type]" class="icon"></span>{{item.name}}
		      </span>
		      </li>
		    </ul>
		</div>
	    <div class="foods-wrapper" ref="foodsWrapper">
	        <ul>
	          <li v-for="item in goods" class="food-list" ref="foodList">
	            <h1 class="title">{{item.name}}</h1>
	            <ul>
	              <li @click.stop.prevent="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
	                <div class="icon">
	                  <img width="57" height="57" :src="food.icon">
	                </div>
	                <div class="content">
	                  <h2 class="name">{{food.name}}</h2>
	                  <p class="desc">{{food.description}}</p>
	                  <div class="extra">
	                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
	                  </div>
	                  <div class="price">
	                    <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
	                  </div>
	                  <div class="cartcontrol-wrapper">
	                    <cartcontrol @add="addFood" :food="food"></cartcontrol>
	                  </div>
	                </div>
	              </li>
	            </ul>
	          </li>
	        </ul>
	    </div>
      <shopcart ref="shopcart" :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice"
                :minPrice="seller.minPrice"></shopcart>
    </div>
    <food @add="addFood" :food="selectedFood" ref="food"></food>
  </div>
</template>



<!-- <template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li  v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
					<span class="text border-2px">
						<span v-show="item.type>0" :class="item.type" class="icon"></span>
							{{item.name}}
					</span>
				</li>
			</ul>
		</div>

			
		<div class="foods-wrapper" ref="foodWrapper">
			<ul>
				<li v-for="item in goods" class="food-list" ref="foodList">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
							<div class="icon">
								<img :src="food.icon" width="57" height="57">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">
										月售{{food.sellCoutn}}份
									</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">
										¥{{food.price}}
									</span>
									<span class="old" v-show="food.oldPrice">
										¥{{food.oldPrice}}
									</span>
								</div>

								<div class="cartcontrol-wrapper">
									<cartcontrol @add="addFood" :food="food"></cartcontrol>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>

		<shopcart ref="shopcart" :deliveryPrice="seller.deliveryPrice"
                :minPrice="seller.minPrice" :select-foods="selectFoods">
			
		</shopcart>

		<div>
			<food :food="selectedFood" ref="food" @add="addFood"></food>
		</div>
	</div>
</template> -->


<script>
import BScroll from 'better-scroll';
import shopcart from '@/components/shopcart/shopcart';
import cartcontrol from '@/components/cartcontrol/cartcontrol';
import food from '@/components/food/food';

const ERR_OK = 0;
export default{
	props:{
		seller:{
			type:Object
		}
	},
	data(){
		return{
			goods:[],
			listHeight:[],
			scrollY:0,
			selectedFood:{}
		}
	},
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
	            return i;
          }
          console.log(height1);
        }
        return 0;
      },
      selectFoods() {
         let foods = [];
         this.goods.forEach((good) => {
           good.foods.forEach((food) => {
             if (food.count) {
               foods.push(food);
             }
           });
         });
         return foods;
       }
     },
	created(){
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

		const url = '/api/goods';
		this.$http.get(url).then((resp)=>{
			resp=resp.body;
			if(resp.errno===ERR_OK){
				this.goods=resp.data;
				/*
				nextTick()的回调对改变的dom进行渲染*/
				this.$nextTick(()=>{
					this._initScroll();
					this._calculateHeight();
				})
			}
		})
	},
	methods:{
		selectFood(food,event){
			if(!event._constructed){
				return;
			}
			this.selectedFood = food;
			this.$refs.food.show();
			console.log(food);
		},
		addFood(target){
			this._drop(target);
			console.log(target+"target");
		},
		_drop(target){
			console.log(target);
			this.$nextTick(()=>{
				this.$refs.shopcart.drop(target);
				// this.$refs.food.show();
			});
		},
		_initScroll(){
			this.meunScroll = new BScroll(this.$refs.menuWrapper,{click:true});
			this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
				click:true,
				probeType:3
			});
		this.foodsScroll.on('scroll',(pos)=>{
			if(pos.y<=0){
				this.scrollY = Math.abs(Math.round(pos.y));
				console.log(this.scrollY);
				console.log(pos);
			}
		});
		},
		_calculateHeight(){
			let foodList = this.$refs.foodList;
			console.log(foodList);
			let height = 0;
			this.listHeight.push(height);
			for(let i=0;i<foodList.length;i++){
				let item = foodList[i];
				height += item.clientHeight;
				this.listHeight.push(height);
			}
			console.log(this.listHeight.length);
		},
		selectMenu(index){
			/*
				在pc端出现调用两次(重新赋值下标造成)selectMenu
				解决：
				接收$event事件

			*/
			// if (!event._constructed){
			// 	return;
			// }
			console.log(index);
			let foodList = this.$refs.foodList;
			let el = foodList[index];
			this.foodsScroll.scrollToElement(el,300);
		}
	},
	components:{
		shopcart,
		cartcontrol,
		food
	},

	// }
}
</script>

<style>
.border-2px{
	border-bottom: 1px solid #E3E5f7;
}
.goods{
	display: flex;
	position: absolute;
	top: 176px;
	width: 100%;
	overflow: hidden;
	bottom: 46px;
}
.goods .menu-wrapper{
	flex: 0 0 80px;
	width: 80px;
	background:#f3f5f7;
}
.goods .menu-wrapper .menu-item{
	display: table;
	height: 54px;
	width: 56px;
	padding:0 12px;
	line-height: 14px;
}
.goods .menu-wrapper .current{
	position: relative;
	z-index: 10;
	margin-top: -1px;
	background: #fff;
	font-weight: 700;
}
.goods .menu-wrapper .menu-item .text{
	display: table-cell;
	width: 56px;
	vertical-align: middle;
	position: relative;
	font-size: 12px;
}
.menu-wrapper .decrease{
	background: url(decrease_3@2x.png);
}
.menu-wrapper .discount{
	background: url(discount_3@2x.png);
}
.menu-wrapper .guarantee{
	background: url(guarantee_3@2x.png);
}
.menu-wrapper .invoice{
	background: url(invoice_3@2x.png);
}
.menu-wrapper .special{
	background: url(special_3@2x.png);
}
.menu-wrapper .icon{
	display: inline-block;
	vertical-align: top;
	width: 12px;
	height: 12px;
	margin-right: 2px;
	background-size: 12px 12px;
	background-repeat: no-repeat;
}
.goods .foods-wrapper{
	flex: 1;
}
.goods .foods-wrapper .food-item{
	display: flex;
	margin:18px;
	padding-bottom: 18px;
	position: relative;
}

.goods .foods-wrapper .food-item:after {
    display: block;
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    border-top: 1px solid rgba(7,17,27,0.1);
    content: ' ';
}
.goods .foods-wrapper .title{
	padding-left: 14px;
	height: 26px;
	line-height: 26px;
	border-left: 2px solid #d9dde1;
	font-size: 12px;
	color: rgb(147,153,159);
	background:#f3f5f7;
}
.goods .foods-wrapper .icon{
	flex:0 0 57px;
	margin-right: 10px;
}
.goods .foods-wrapper .content{
	flex: 1;
}
.goods .foods-wrapper .content .name{
	margin:2px 0 8px 0;
	/*height: 14px;*/
	line-height: 14px;
	font-size: 14px;
	color: rgb(7,17,27);
}
.goods .foods-wrapper .content .desc,.extra{
	line-height: 10px;
	font-size: 10px;
	color: rgb(147,153,159);
}
.goods .foods-wrapper .content .desc{
	line-height: 12px;
	margin-bottom: 8px;
}
.goods .foods-wrapper .content .extra .count{
	margin-right: 12px;
}
.goods .foods-wrapper .content .price{
	font-weight: 700;
	line-height: 24px;
}
.goods .foods-wrapper .content .cartcontrol-wrapper{
	position: absolute;
	right: 0;
	bottom: 12px;
}
.goods .foods-wrapper .content .price .now{
	margin-right: 8px;
	font-size: 14px;
	color: rgb(240,20,20);
}
.goods .foods-wrapper .content .price .old{
	text-decoration: line-through;
	font-size: 10px;
	color: rgb(147,153,159);
}
</style>