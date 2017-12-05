<template>
	<div class="cartcontrol">
		<div class="cart-descrease" v-show="food.count>0" @click.stop.prevent="descreaseCart" translate="move"> 
			<span class="circle">-</span>
		</div>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<transition name="move">
			<div class="cart-add" @click.stop.prevent="addCart($event)">
					<span class="circle">+</span>
			</div>
		</transition>
	</div>
</template>

<script> 
import Vue from 'vue';
	export default{
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			addCart(event){
				if(!event._constructed){
					return;
				}
				if(!this.food.count){
					Vue.set(this.food,'count',1);
				}else{
					this.food.count++;
				}
				console.log(this.food.count);
				// this.$dispathch('cart.add',event.target);
				this.$emit('add',event.target);
			},
			descreaseCart(){
				if(!event._constructed){
					return;
				}
				if(this.food.count){
					this.food.count--;
				}
			}
		}
	}
</script>

<style scoped>
.cartcontrol{
	font-size: 0;
}
.cartcontrol .cart-count{
	display: inline-block;
	vertical-align: top;
	width: 12px;
	padding: 6px;
	line-height: 24px;
	text-align: center;
	font-size: 10px;
	color: rgb(147,153,159);
}
.cartcontrol .cart-add,.cartcontrol .cart-descrease{
	display: inline-block;
	padding: 6px;
	line-height: 24px;
	font-size: 24px;
	color:rgb(0,160,220);
	transition: all 0.4s linear;
}
.cartcontrol .cart-add .circle,.cartcontrol .cart-descrease .circle{
	display: inline-block;
	color: #fff;
	background: rgb(0,160,220);
	border-radius: 50%;
	transition: all 0.4s linear;
	transform: rotate(0);

}

.cartcontrol .cart-descrease .move-enter , .cartcontrol .cart-descrease .move-leave{
	 opacity: 0;
	 transform: translate3d(24px,0,0);
}
.cartcontrol .cart-descrease .move-enter , .cartcontrol .cart-descrease .move-leave{
	 transform: rotate(180deg);
	
}
.cartcontrol .cart-descrease .move-transition{
	opacity: 1;
	transform:translate3d(0,0,0);
}

</style>