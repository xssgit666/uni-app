<template>
	<view class='commodity'>
        <!-- 单个商品组件 -->		
		<view class='commodity-item' 
			v-for="(item,index) in dataList"
			:key='index'
		>
			<image class='commodity-img' :src="item.imgUrl"></image>
			<view class='commodity-content'>
				<text class='commodity-name'>{{item.name}}</text>
				<view class="commodity-price">
					<text class='pprice'>¥{{item.pprice}}</text>
					<text class='oprice'>¥{{item.oprice}}</text>
				</view>
				<text class='discount'>{{item.discount}}折</text>
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
.commodity{
	padding: 20rpx;
	display: flex;
	flex-wrap: nowrap;
}
.commodity-item{
	width: 200rpx;
	margin-right: 20rpx;
}
.commodity-item:last-child{
	margin-right: 0;
}
.commodity-img{	
	border-radius: 20rpx 20rpx 0 0;
	width:100%;
	display: block;
	height: 200rpx;
}
.commodity-content{
	text-align: left;
	border-radius: 0 0 20rpx 20rpx;
	background-color: #FFFFFF;
	padding: 10rpx 20rpx;
}
.commodity-name{
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp:1;
	-webkit-box-orient:vertical;
	color:#333333;
	word-break: break-all;
	text-align: justify;
	font-size: 26rpx;
	margin-bottom: 10rpx;
}
.commodity-price{
	line-height: 1;
}
.pprice{
	font-size: 28rpx;
	color:#FF3333;
}
.oprice{
	margin-left: 10rpx;
	text-decoration: line-through;
	font-size:24rpx;
	color:#999999;
}
.discount{
	color:#FF3333;
	border-radius: 4rpx;
	border:1px solid #FF3333;
	padding:0 6rpx;
	font-size: 20rpx;
}
</style>