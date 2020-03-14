<template>
	<view>
		
		<view class="mglr4">
			<view class="vipCard mgt15 fs15 radius10 oh">
				<view class="pic"><image src="../../static/images/vip-img.png" mode=""></image></view>
				<view class="tit pdt15 pdb10">到期时间：2020-01-01至2020-12-30</view>
			</view>
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
	page{padding-bottom:60rpx;}
	
	.vipCard .pic{width: 100%;height: 433rpx;}
	.vipCard .pic image{width: 100%;height: 100%;}
	
</style>
