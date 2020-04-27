<template>
	<view>
		
		<view class="mglr4">
			<view class="vipCard mgt15 fs15 radius10 oh">
				<view class="pic"><image src="../../static/images/vip-img.png" mode=""></image></view>
				<view class="tit pdt15 pdb10" v-if="userInfoData.is_member==0">您还不是会员身份</view>
				<view class="tit pdt15 pdb10" v-else>到期时间：{{userInfoData.start_time}}至{{userInfoData.end_time}}</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
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
	page{padding-bottom:60rpx;}
	
	.vipCard .pic{width: 100%;height: 433rpx;}
	.vipCard .pic image{width: 100%;height: 100%;}
	
</style>
