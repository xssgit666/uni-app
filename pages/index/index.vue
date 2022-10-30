<template>
	<view class="index">
		<!-- 顶部滑块 -->
		<scroll-view
			scroll-x="true" 
			class='scroll-content' 
			:scroll-into-view='scrollIntoIndex' 
			scroll-with-animation="true"
			show-scrollbar="false"
		>
			<view
				class='scroll-item'
				v-for='(item,index) in topBar'
				:key='index'
				:id="'top'+index"
				@tap='changeTab(index)'
			>
				<text :class='topBarIndex===index? "f-active-color":"f-color"'>{{item.name}}</text>
			</view>
		</scroll-view>
		
		<!-- 内容部分 -->
		<swiper @change='onChangeTab' :current="topBarIndex" :style="'height:'+clentHeight+'px;'">
			<swiper-item 
				v-for='(item,index) in newTopBar'
				:key='index'
			>
				<scroll-view scroll-y="true"  :style="'height:'+clentHeight+'px;'" @scrolltolower='loadMore(index)'>
					<block v-if='item.data.length > 0 '>
						<block v-for='(k,i) in item.data' :key='i'>
							
							<!-- 推荐 -->
							<IndexSwiper v-if='k.type==="swiperList"' :dataList='k.data'></IndexSwiper>
							<template v-if='k.type==="recommendList"' >
								<Recommend :dataList='k.data'></Recommend>
								<Card cardTitle='猜你喜欢'></Card>
							</template>
							
							<!--运动户外....-->
							<Banner v-if='k.type==="bannerList"' :dataList='k.imgUrl'></Banner>
							
							<template v-if='k.type==="iconsList"'>
								<Icons :dataList='k.data'></Icons>
								<Card cardTitle='热销爆品'></Card>
							</template>
							
							<template v-if='k.type==="hotList"'>
								<Hot :dataList='k.data'></Hot>
								<Card cardTitle='推荐店铺'></Card>
							</template>
							
							<template v-if='k.type==="shopList"'>
								<Shop :dataList='k.data'></Shop>
								<Card cardTitle='为您推荐'></Card>
							</template>
							
							<CommodityList v-if='k.type==="commodityList"' :dataList='k.data'></CommodityList>
							
						</block>
					</block>
					<view v-else>
						暂无数据...
					</view>
					<view class='load-text f-color'>
						{{item.loadText}}
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
		
		<!-- 推荐模版 -->
		<!-- <IndexSwiper></IndexSwiper>
		<Recommend></Recommend>
		<Card cardTitle="猜你喜欢"></Card>
		<CommodityList></CommodityList> -->
		
		<!-- 其他模版：运动户外、美妆... -->
		<!-- <Banner></Banner>
		<Icons></Icons>
		<Card cardTitle='热销爆品'></Card>
		<Hot></Hot>
		<Card cardTitle='推荐店铺'></Card>
		<Shop></Shop>
		<Card cardTitle='为您推荐'></Card>
		<CommodityList></CommodityList> -->
	</view>
</template>

<script>
	import $http from '@/common/api/request.js'
	import IndexSwiper from '@/components/index/IndexSwiper.vue'
	import Recommend from '@/components/index/Recommend.vue'
	import Card from '@/components/common/Card.vue'
	import CommodityList from '@/components/common/CommodityList.vue'
	import Banner from '@/components/index/Banner.vue'
	import Icons from '@/components/index/Icons.vue'
	import Hot from '@/components/index/Hot.vue'
	import Shop from '@/components/index/Shop.vue'
	export default {
		data() {
			return {
				//选中的索引
				topBarIndex:0,
				//顶栏跟随的索引id值
				scrollIntoIndex:'top0',
				//内容块的高度值
				clentHeight:0,
				//顶栏数据
				topBar:[],
				//承载数据
				newTopBar:[]
			}
		},
		components: {
			IndexSwiper,
			Recommend,
			Card,
			CommodityList,
			Banner,
			Icons,
			Hot,
			Shop
		},
		onLoad() {
			this.__init();
		},
		onReady() {
			uni.getSystemInfo({
				success: (res) => {
					// this.clentHeight = res.windowHeight - uni.upx2px(80)-this.getClientHeight();
					this.clentHeight = res.windowHeight - this.getClientHeight();
				}
			})
		},
		// 页面生命周期-(标题栏按钮点击)
		onNavigationBarButtonTap(e){
			if(e.float=='left'){
				uni.navigateTo({ // 保留当前页面，跳转到应用内的某个页面
					url:'../search/search',
					animationType: 'slide-in-right',
					animationDuration: 300
				})
			}
		},
		methods:{
			// 加载数据
			__init(){
				$http.request({
					url:"/index_list/data"
				}).then((res)=>{
					this.topBar = res.topBar;
					this.newTopBar = this.initData(res);
				}).catch(()=>{
					uni.showToast({
						title:'请求失败',
						icon:'error'
					})
				})
			},
			// 添加数据
			initData(res){
				let arr = [];
				for(let i =0;i<this.topBar.length;i++){
					let obj = {
						data:[],
						load:"first", // 标记滑动
						loadText:"上拉加载更多..."
					}
					// 获取首次数据
					if(i==0){
						obj.data = res.data;
					}
					arr.push(obj)
				}
				return arr;
			},
			// 点击顶栏
			changeTab(index){
				if(this.topBarIndex === index){
					return ;
				}
				this.topBarIndex = index;
				this.scrollIntoIndex = 'top'+index;
				
				// 只有第一次滑动到才请求加载
				if( this.newTopBar[this.topBarIndex].load  ==='first'){
					this.addData();
				}
			},
			// 对应滑动
			onChangeTab(e){
				this.changeTab(e.detail.current);
			},
			//获取可视区域高度
			getClientHeight(){
				const res = uni.getSystemInfoSync();
				// 【兼容】
				// const system = res.platform;
				// if( system ==='ios' ){
				// 	return 44+res.statusBarHeight;
				// }else if( system==='android' ){
				// 	return 48+res.statusBarHeight;
				// }else{
				// 	return 0;
				// }
				return res.statusBarHeight;
			},
			//对应显示不同数据
			addData(callback){
				// 索引
				let index = this.topBarIndex;
				// id
				let id = this.topBar[index].id;
				// 请求不同的数据
				let page = Math.ceil(this.newTopBar[index].data.length / 5) + 1;
				
				//请求数据
				$http.request({
					url:'/index_list/'+id+'/data/'+page+''
				}).then((res)=>{
					this.newTopBar[index].data = [...this.newTopBar[index].data,...res];
				}).catch(()=>{
					this.newTopBar[index].loadText = '没有更多了';
					uni.showToast({
						title:'没有更多了',
						icon:'error'
					})
				})
				
				// 当请求结束后，赋值last，后续不再请求
				this.newTopBar[index].load='last';
				
				if(typeof callback==='function'){
					callback();
				}
			},
			// 上拉加载更多
			loadMore(index){				
				this.newTopBar[index].loadText = '加载中...';
				// 请求完数据 ，文字提示信息换成【上拉加载更多...】
				this.addData(()=>{ // 作为实参传给请求函数
					this.newTopBar[index].loadText = '上拉加载更多...';
				})
			}
		}
	}
</script>

<style>
	.index{
		background-color: aliceblue;
	}
	.scroll-content{
		width: 100%;
		height: 80rpx;
		white-space: nowrap;
	}
	.scroll-item{
		display: inline-block;
		padding:10rpx 20rpx;
		font-size:32rpx;
	}
	.f-active-color{
		padding:10rpx 0;
		font-weight: bold;
		border-bottom:6rpx solid #49BDFB;
	}
	.load-text{
		margin: 0 0 20rpx;
		text-align: center;
	}
</style>
