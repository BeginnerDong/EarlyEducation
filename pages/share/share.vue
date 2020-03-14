<template>
	<view>
		
		<view class="backBtn" @click="prev()">
			<image src="../../static/images/arrowL.png" mode=""></image>
		</view>
		<view class="sharePic">
			<image class="pic" src="../../static/images/share-img.png" mode=""></image>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			prev(){
				const self = this;
				self.$router.go(-1)
			},
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
	.backBtn{padding: 20rpx 30rpx;position: fixed;top: 40rpx;left: 0rpx; z-index: 2;}
	.backBtn image{width: 20rpx;height:36rpx;}
	.sharePic{position: fixed;top: 0rpx;left:0rpx;right: 0;bottom: 0; z-index: 0;}
	.sharePic .pic{width: 100%;height: 100%;}
</style>
