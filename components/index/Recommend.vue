<template>
	<view class="recommend">
		<view class='recommend-item' v-for='(item,index) in dataList' :key='index'>
			<image class='item-big' :src="item.bigUrl" mode=""></image>
			<view class='item-small'>
				<block v-for='(k,i) in item.data' :key='i'>
					<image class='item-img' :src="k.imgUrl" mode=""></image>
				</block>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			dataList:Array
		}
	}
</script>

<style scoped>
	.recommend{
		padding:0 20rpx;
	}
	.recommend-item{
		border-radius: 20rpx;
		/* border: 2rpx solid #CCCCCC; */
		overflow: hidden;
		margin:20rpx 0;
	}
	.item-big{
		display: block;
		width:100%;
		height: 300rpx;
	}
	.item-small{
		width:100%;
		height: 240rpx;
	}
	.item-img{
		width:33.3333%;
		height: 240rpx;
	}
</style>