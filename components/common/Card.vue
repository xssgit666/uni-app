<template>
	<view class="card">
		<span>-</span>
		<slot>
			<span class='card-name'>{{cardTitle}}</span>
		</slot>
		<span>-</span>
	</view>
</template>

<script>
	export default {
		props:{
			cardTitle:String
		}
	}
</script>

<style scoped>
	.card{
		padding:20rpx 0;
		text-align: center;
		font-weight: bold;
	}
	.card-name{
		padding:0 20rpx;
		font-size:32rpx;
	}
</style>