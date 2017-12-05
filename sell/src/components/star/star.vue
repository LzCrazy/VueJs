<template>
	<div class="star" :class=
	starType>
		<span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
	</div>
</template>

<script>
	export default{
		props:{
			size:{
				stype:Number
			},
			score:{
				type:Number
			}
		},
		computed:{
			starType(){
				return 'star-'+this.size
			}
		}
	}
</script>

<style>
	.star{
		display: inline-block;
		background-repeat: no-repeat;
	}
	.star .star-item{

	}
</style>