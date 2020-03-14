<template>
	<view>
		
		<view class="foundList">
			<view class="item" v-for="(item,index) in proData" :key="index" @click="Router.navigateTo({route:{path:'/pages/foundDetail/foundDetail'}})">
				<view class="flexRowBetween fs13">
					<view class="flex pdb10">
						<view class="photo">
							<image src="../../static/images/found-img.png" mode=""></image>
						</view>
						<view>小萌姐</view>
					</view>
					<view class="color9">2020-03-13</view>
				</view>
				<view class="video">
					<image src="../../static/images/found-img1.png"></image>
				</view>
				<view class="infor center flexEnd fs12 pdt15">
					<view class="sBtn flex">
						<view><image class="icon" src="../../static/images/found-icon1.png" mode=""></image></view>
						<view class="">123</view>
					</view>
					<view class="sBtn flex">
						<view><image class="icon" src="../../static/images/found-icon2.png" mode=""></image></view>
						<view class="">520</view>
					</view>
					<view class="sBtn flex">
						<view><image class="icon" src="../../static/images/found-icon4.png" mode=""></image></view>
						<view class="">563</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="R-fixIcon" @click="Router.navigateTo({route:{path:'/pages/found-camera/found-camera'}})"><image src="../../static/images/found-icon.png" mode=""></image></view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/found/found'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">发现</view>
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
				is_show: false,
				wx_info:{},
				is_show:false,
				proData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	/* @import "../../assets/style/proList.css"; */
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	
	.foundList .item{padding: 30rpx 4%;background: #fff;margin-bottom:20rpx;}
	.foundList .item .video{width: 100%;height: 360rpx;border-radius: 10rpx;overflow: hidden;}
	.video image{width: 100%;height: 100%;}
	
	.foundList .photo{width: 60rpx;height: 60rpx;border-radius: 50%;margin-right: 10rpx;overflow: hidden;}
	.foundList .photo image{width: 100%; height: 100%;}
	
	.foundList .item .infor .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.sBtn{margin-left: 100rpx;}
	
	.R-fixIcon{width: 90rpx;height: 90rpx;position: fixed;top: 48%;right: 30rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
</style>
