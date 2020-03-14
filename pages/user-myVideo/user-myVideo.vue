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
				proData:[{},{}]
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
	
	page{padding-bottom: 60rpx;background: #F5F5F5;}	
	
	.foundList .item{padding: 30rpx 4%;background: #fff;margin-bottom: 20rpx;}
	.foundList .item .video{width: 100%;height: 360rpx;border-radius: 10rpx;overflow: hidden;}
	.video image{width: 100%;height: 100%;}
	
	.foundList .photo{width: 60rpx;height: 60rpx;border-radius: 50%;margin-right: 10rpx;overflow: hidden;}
	.foundList .photo image{width: 100%; height: 100%;}
	
	.foundList .item .infor .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.sBtn{margin-left: 100rpx;}
	
</style>
