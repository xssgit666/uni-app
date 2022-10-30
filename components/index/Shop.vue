<template>
	<view class='shop'>
		<view class="shop-item" v-for='(item,index) in dataList' :key='index'>
			<image class='shop-big' :src="item.bigUrl"></image>
			<scroll-view scroll-x="true" class='scroll-content'>
				<view class='scroll-item'>
					<CommodityFour :dataList='item.data'></CommodityFour>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import CommodityFour from '../common/CommodityFour.vue'
	export default {
		props:{
			dataList:Array
		},
		components:{
			CommodityFour
		}
	}
</script>

<style scoped>
.shop-big{
	width:100%;
	height: 350rpx;
	display: block;
}
.scroll-content{
	width: 100%;
	height: auto; /* 为苹果加的高 */
	white-space: nowrap;
}
.scroll-item{	
	padding: 0; /* 为苹果加的0内边距 */
	display: inline-block;
}
</style>