<template>
	<view>
		
		<view class="pdlr4 pdt15">
			<textarea value="" placeholder="写标题并使用合适的话题" placeholder-class="placeholder" />
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdt15">
			<view class="flex videoList">
				<view class="item"><image src="../../static/images/release-img1.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button">发布</button>
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
	textarea{padding: 0;height: 360rpx;}
	.videoList{flex-wrap: wrap;}
	.videoList .item{width: 150rpx;height: 180rpx;margin:0 30rpx 30rpx 0;}
	.videoList .item image{width: 100%;height: 100%;}
	.videoList .item:nth-of-type(4n){margin-right: 0;}
</style>
