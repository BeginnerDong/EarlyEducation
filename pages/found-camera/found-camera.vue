<template>
	<view>
		
		<view class="cameraBtn" @click="Router.navigateTo({route:{path:'/pages/found-fabu/found-fabu'}})"></view>
		
		<view class="uploadBtn flexColumn fs12" @click="Router.navigateTo({route:{path:'/pages/found-fabu/found-fabu'}})">
			<view><image src="../../static/images/release-icon.png" mode=""></image></view>
			<view class="mgt5">上传</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false
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
	
	.cameraBtn{width: 120rpx;height: 120rpx;border-radius: 50%;background: #42D040;border: 4rpx solid #fff;position: fixed;bottom:100rpx;left: 50%;transform: translateX(-50%);box-shadow: 0 0 14px rgb(55, 255, 19);}	
	.uploadBtn{position: fixed;bottom: 110rpx;right:12%;}
	.uploadBtn image{width: 60rpx;height: 60rpx;}
</style>
