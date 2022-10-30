<template>
	<view class='hot'>
		<CommodityThree :dataList='dataList'></CommodityThree>
	</view>
</template>

<script>
	import CommodityThree from '../common/CommodityThree.vue'
	export default {
		props:{
			dataList:Array
		},
		components: {
			CommodityThree
		}
	}
</script>

<style>
</style>