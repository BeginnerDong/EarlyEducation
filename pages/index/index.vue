<template>
	<view>
		
		<view class="">
			<view class="banner-box pdlr4">
				<view class="banner">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#ff5e43">
						<block v-for="(item,index) in bannerData" :key="index">
							<swiper-item class="swiper-item" :data-url="item.url"
							  @click="Router.navigateTo({route:{path:$event.currentTarget.dataset.url}})">
								<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image"/>
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj mglr4 pdtb15 fs12">
			<view class="item" v-for="(item,index) in labelData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="fs15 pdt5 ftw">推荐课程</view>
			<view class="proList flexRowBetween" style="min-height: 700rpx;">
				<view class="item boxShaow whiteBj" v-for="(item,index) in hotData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})">
					<view class="video">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						<view class="time flex"><image class="sIcon" src="../../static/images/home-icon5.png" mode=""></image>{{item.keywords}}</view>
					</view>
					<view class="infor center">
						<view class="tit avoidOverflow">{{item.title}}</view>
						<view class="text fs12 color9 avoidOverflow">{{item.description}}</view>
					</view>
				</view>
			</view>
			
			<view class="fs15 pdt15 ftw">最新视频</view>
			<view class="flex newVideo mgb20">
				<view class="item boxShaow" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/detail/detail?id='+$event.currentTarget.dataset.id}})">
					<view class="video">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"mode=""></image>
						<view class="time flex"><image class="sIcon" src="../../static/images/home-icon5.png" mode=""></image>{{item.keywords}}</view>
					</view>
					<view class="infor center">
						<view class="tit fs13 avoidOverflow">{{item.title}}</view>
						<view class="text fs11 color9 avoidOverflow">{{item.description}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/found/found'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">发现</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				bannerData: [
				],
				labelData:[],
				idArray:[],
				mainData:[],
				hotData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getLabelData','getBannerData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			getBannerData() {
				const self = this;
				self.bannerData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', ['轮播图']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.bannerData.push.apply(self.bannerData, res.info.data)
					}
					console.log('self.bannerData', self.bannerData)
					self.$Utils.finishFunc('getBannerData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					label:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							title:['in',['课程类别']],
							status:['in',[1]]
						},
						condition:'in'
					}
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.labelData.push.apply(self.labelData, res.info.data);
						for (var i = 0; i < self.labelData.length; i++) {
							self.idArray.push(self.labelData[i].id);
						}
					};
					self.getMainData();
					self.getHotData()
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getHotData() {
				var self = this;
				var postData = {};
				postData.paginate = {
					count: 0,
					currentPage:1,
					pagesize:4,
					is_page:true,
				};
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id:['in',self.idArray]
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.hotData.push.apply(self.hotData, res.info.data);
					};
					self.$Utils.finishFunc('getHotData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					menu_id:['in',self.idArray]
				};
				postData.order = {
					create_time: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;}	
	
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image {width: 100%;height: 100%;box-sizing: border-box;border-radius: 10rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 20%;text-align: center;color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:60rpx;height:60rpx; margin: 0 auto 10rpx auto; }
	
	.newVideo{flex-wrap: wrap;}
	.newVideo .item{width: 220rpx;box-sizing: border-box;margin-top: 20rpx;border-radius:20rpx;overflow: hidden;height:230rpx; margin-right: 15rpx;}
	.newVideo .item:nth-of-type(3n){margin-right: 0;}
	.newVideo .video{width: 100%;height:133rpx;position: relative;}
	.newVideo .video image{width: 100%;height: 100%;}
	.newVideo .video .time{position: absolute;bottom: 0;left: 0;right: 0;background-image: linear-gradient(to bottom, rgba(0,0,0,0) 5%, rgba(0,0,0,0.3) 100%);padding: 10rpx 4%;box-sizing: border-box;font-size: 16rpx;color: #fff;}
	.newVideo .video .time .sIcon{width: 14px;height: 14px;margin-right: 10rpx;}
	.newVideo .item .infor{padding: 10rpx 4%;line-height:38rpx;}
</style>
