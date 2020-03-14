<template>
	<view>
		
		<view class="detail_pjLis pdlr4">
			<view class="item" v-for="(item,index) in messageData" :key="index">
				<view class="photo"><image src="../../static/images/details-img1.png" mode=""></image></view>
				<view class="cont">
					<view class="flexRowBetween pdt5 pdb10 fs13">
						<view class="name color6">哆啦A梦</view>
						<view class="time color9">2020-03-04</view>
					</view>
					<view class="text">根据瑞灵活赶紧来都搞好了电话沟通各环节的反馈给换了多个黑客帝国</view>
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
				showView: false,
				score:'',
				wx_info:{},
				messageData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	@import "../../assets/style/detail.css";
	page{padding-bottom: 60rpx;}
	
	
</style>
