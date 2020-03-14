<template>
	<view>
		
		<view class="pdlr4">
			<view class="proList flexRowBetween">
				<view class="item boxShaow" v-for="(item,index) in proData" :key="index" @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
					<view class="video">
						<image src="../../static/images/home-img.png" mode=""></image>
						<view class="time flex"><image class="sIcon" src="../../static/images/home-icon5.png" mode=""></image>03:09</view>
					</view>
					<view class="infor center">
						<view class="tit avoidOverflow">经典中文儿歌</view>
						<view class="text fs10 color9 avoidOverflow">最受欢迎的而她给你歌曲</view>
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
				proData:[{},{},{},{},{},{},{},{}]
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
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 60rpx;}	
	
</style>
