<template>
	<view>
		
		<view class="mglr4">
			<view class="vipCard center mgt15 fs15 radius10 oh">
				<view class="pic"><image src="../../static/images/vip-img.png" mode=""></image></view>
				<view class="tit pdt15 pdb10">到期时间：2020-01-01至2020-12-30</view>
			</view>
		</view>
		
		<view class="xqbotomBar pdl15" style="height: 100rpx;">
			<view class="flex fs13">总计：<view class="price fs16 ftw">299</view></view>
			<view class="payBtn flexCenter" style="height: 100rpx;">确认订单</view>
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
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.vipCard .pic{width: 100%;height: 433rpx;}
	.vipCard .pic image{width: 100%;height: 100%;}
	
</style>
