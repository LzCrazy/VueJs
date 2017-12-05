<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" width="64" height="64">
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
					<span class="icon"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<div v-if="seller.supports" class="support-count" @click="showDetail">
				<span class="count">5个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span>
			<span class="bulletin-text">
				{{seller.bulletin}}
			</span>
			<i class="icon-keyboard-arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>

		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix">
				<div class="detail-main">
					<h1 class="name">{{seller.name}}</h1>
					<div class="star-wrapper">
						<!-- <star :size="48" :score="seller.score"></star> -->
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
					<ul class="supports">
						<li class="support-item" v-for="(item,index) in seller.supports">
							<span class="icon"></span>
							<span class="text">
								{{seller.supports[index].description}}
							</span>
						</li>
					</ul>
					<div class="title">
						<div class="line"></div>
						<div class="text">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="bulletin">
						<p class="content">
							{{seller.bulletin}}
						</p>
					</div>
				</div>
			</div>
			<div class="detail-close" @click="hideDetail">
				<i class="icon"></i>
			</div>
		</div>
	</div>
</template>

<script>
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				detailShow:false
			}
		},
		methods:{
			showDetail(){
				this.detailShow = true;
			},
			hideDetail(){
				this.detailShow = false;
			}
		},
		created(){
			this.classMap=['decrease','discount','special','invoice','guarantee'];
		}
	}
</script>

<style>
	.clearfix{
		display: inline-block;
	}
	.clearfix:after{
		display: block;
		content: ".";
		height: 0;
		line-height: 0;
		clear: both;
		visibility: hidden;
	}

	.header{
		position: relative;
		overflow: hidden;
		background:rgba(7,17,27,0.5);
		color:#fff;
	}
	.content-wrapper{
		position: relative;
		padding:24px 14px 18px 24px;
		font-size: 0;
	}
	.avatar{
		display: inline-block;
		vertical-align: top;
	}
	.avatar img{
		border-radius: 2px;
	}
	.content{
		display:inline-block;
		margin-left: 16px;
	}
	.content .title{
		margin:2px 0 8px 0;
	}
	.content .title .brand{
		display: inline-block;
		vertical-align: top;
		width: 30px;
		height: 18px;
		background-size: 30px 18px;
		background-repeat: no-repeat;
		background-image: url(brand@2x.png);
	}
	.content .title .name{
		margin-left: 6px;
		font-size: 16px;
		line-height: 18px;
		font-weight: bold;	
	}
	.description{
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;
	}
	.support .icon{
	   display: inline-block;
       width: 16px;
       height: 16px;
       vertical-align: top;
       margin-right: 6px;
       background-size: 16px 16px;
       background-repeat: no-repeat;
		background-image: url(decrease_1@2x.png);
	}
	.support .text{
		line-height: 12px;
		font-size:10px;
	}
	.support-count{
		position: absolute;
		right: 12px;
		bottom: 18px;
		height: 24px;
		padding: 0 10px;
		line-height: 24px;
		border-radius: 14px;
		background:rgba(0,0,0,0.5); 
		text-align: center;
	}
	.support-count .count{
		vertical-align: top;;
		font-size: 10px;
	}
	.support-count .icon-keyboard_arrow-right{
		margin-left: 2px;
		line-height: 24px;
		font-size: 10px;
	}
	.bulletin-wrapper{
		position: relative;
		height: 28px;
		line-height: 28px;
		padding:0 22px 0 12px;
		white-space: nowrap;
		text-overflow: ellipsis;
		background:rgba(7,17,27,0.3);
	}
	.bulletin-wrapper .bulletin-title{
		display: inline-block;
		vertical-align: top;
		margin-top: 8px;
		width: 22px;
		height: 12px;
		background-image: url(bulletin@2x.png);
		background-size: 22px 12px;
		background-repeat: no-repeat;
	}
	.bulletin-wrapper .bulletin-text{
		vertical-align: top;
		margin:0 4px;
		font-size: 10px;
	}
	.bulletin-wrapper .icon-keyboard_arrow_right{
		position: absolute;
		font-size: 10px;
		right: 12px;
		top: 8px;
	}
	.background{
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100$;
		z-index: -1;
		filter: blur(10px);
	}
	.detail{
		position: fixed;
		z-index: 100;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		overflow: auto;
	    backdrop-filter: blur(10px);
	    opacity: 1;
	    background: rgba(7,17,27,0.8);
	}
	.detail-wrapper{
		width: 100%;
		min-height: 100%;
	}
	.detail-wrapper .detail-main{
		margin-top: 64px;
		padding-bottom: 64px;
	}
	.detail-wrapper .detail-main .name{
		line-height: 16px;
		text-align: center;
		font-size: 16px;
		font-weight: 700
	}
	.detail-wrapper .detail-main .star-wrapper{
		margin-top: 18px;
		padding:2px 0;
		text-align: center;
	}
	.detail-wrapper .detail-main .title{
		display: flex;
		width: 80%;
		margin:28px auto 24px auto;
	}
	.detail-wrapper .detail-main .line{
		flex: 1;
		position: relative;
		top: -6;
		border-bottom: 1px solid rgba(255,255,255,0.2);
	}
	.detail-wrapper .detail-main .text{
		padding:0 12px;
		font-weight: 700;
		font-size:14px;
	}
	.detail-close{
		position: relative;
		width: 32px;
		height: 32px;
		margin:-64px auto 0 auto;
		clear: both;
		font-size: 32px;
	}
	.detail-close i{
		display: block;
		width: 35px;
		height: 35px;
		background-image: url(x.png);
	}
	.supports{
		width: 80%;
		margin:0 auto;
	}
	.supports .support-item{
		padding:0 12px;
		margin-bottom: 12px;
		font-size: 0;
	}
	.supports .support-item:last-child{
		margin-bottom: 0;
	}
	.supports .support-item .icon{
		display: inline-block;
		width: 16px;
		height: 16px;
		vertical-align: top;
		margin-right: 6px;
		background-size: 16px 16px;
		background-repeat: no-repeat;
		background-image: url(decrease_1@2x.png);
	}
	.supports .support-item .text{
		line-height: 16px;
		font-size: 12px;
	}
	.detail-wrapper .detail-main .bulletin{
		width: 80%;
		margin:0 auto;
	}
	.detail-wrapper .detail-main .bulletin .content{
		padding:0 12px;
		line-height: 24px;
		font-size: 12px;
	}

</style>