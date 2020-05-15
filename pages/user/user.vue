<template>
	<view>
		<view class="headbj"><image src="../../static/images/about-icon01.png" mode=""></image></view>
		<view class="userHead white">
			<view class="infor flex pdt20">
				<view class="photo" style="overflow: hidden;">
					<open-data type="userAvatarUrl"></open-data>
				</view>
				<view style="width: 520rpx;">
					<view class="fs16 ftw"><open-data type="userNickName"></open-data></view>
					<view class="flex mgt10" v-if="userInfoData.is_member>0">
						<view><image style="width: 31rpx;height: 26rpx;margin-right: 10rpx;" src="../../static/images/about-icon0.png" mode=""></image></view>
						<view class="fs12">有效期：{{userInfoData.start_time}}-{{userInfoData.end_time}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="myRowBetween fs13 mglr4 pdt10">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myVideo/user-myVideo'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view>我的视频</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
			</view>
			<!-- <view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myVIP/user-myVIP'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view>我的会员</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
			</view> -->
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-comment/user-comment'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view>我的评论</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myMoney/user-myMoney'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view>我的佣金</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/disclaimer/disclaimer'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
					<view>免责声明</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" v-if="bannerData&&bannerData[0]&&bannerData[0].url!='1'" @click="Router.redirectTo({route:{path:'/pages/found/found'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">发现</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				userInfoData:{},
				bannerData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData','getBannerData'], self);
		},
		
		methods: {
			
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
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.userInfoData.start_time = 	self.$Utils.timeto(self.userInfoData.start_time*1000,'ymd')
						self.userInfoData.end_time = 	self.$Utils.timeto(self.userInfoData.end_time*1000,'ymd')
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 130rpx;}
	
	
	.userHead{width: 100%;box-sizing: border-box;padding: 0 4%;height: 200rpx;position: relative;}
	.headbj{width: 100%;height:200rpx;position: absolute;top: 0;right: 0;bottom: 0;left: 0;z-index: 0;}
	.headbj image{width: 100%;height: 100%;display: block;}
	.userHead .infor{box-sizing: border-box;}
	
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;margin-right: 20rpx;box-sizing: border-box;}
	
	.myRowBetween .ll .icon{width: 30rpx;height: 32rpx;margin-right: 20rpx;}
	
	
</style>
