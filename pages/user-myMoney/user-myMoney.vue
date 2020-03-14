<template>
	<view>
		
		<view class="myMoney pdlr4 pubBj white center">
			<view class="bigNum ftw">5652</view>
			<view class="fs154 mgt10">总佣金</view>
			<view class="fs13" style="margin-top: 40rpx;">已提现金额：￥230</view>
		</view>
		
		<view class="mglr4 pdtb15 flexRowBetween">
			<view class="flex">
				<view class="mgr15">可提现金额</view>
				<view>563元</view>
			</view>
			<view class="txBtn" @click="Router.navigateTo({route:{path:'/pages/user-cashOut/user-cashOut'}})">提现</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="orderNav flexRowBetween pdt5 pdb15 borderB1">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">推广佣金</view>
			<view class="tt flexCenter" :class="curr==2?'on':''"  @click="changeCurr('2')">浏览量收益</view>
		</view>
		
		<view class="myRowBetween fs13 mglr4 " v-show="curr==1">
			<view class="item flexRowBetween" v-for="(item,index) in tuiguangData" :key="index">
				<view class="ll">
					<view class="">推广佣金</view>
					<view class="fs13 color9">2020-3-13</view>
				</view>
				<view class="rr red">+1</view>
			</view>
		</view>
		
		<view class="myRowBetween fs13 mglr4 " v-show="curr==2">
			<view class="item flexRowBetween" v-for="(item,index) in browseData" :key="index">
				<view class="ll">
					<view class="">浏览量</view>
					<view class="fs12 color9">总计：562</view>
				</view>
				<view class="rr red">+1</view>
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
				curr:1,
				tuiguangData:[{},{},{},{}],
				browseData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}

		},
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/editInfor.css";
	
	page{padding-bottom: 130rpx;}
	.myMoney{height: 360rpx;box-sizing: border-box;padding-top: 90rpx;}
	.myMoney .bigNum{font-size: 50rpx;line-height: 50rpx;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
	.txBtn{width: 142rpx;height: 56rpx;line-height: 56rpx;text-align: center;border-radius: 36rpx;color: #fff;background-color: #42D040;box-shadow: 0 4rpx 0 #1cb71a;}
</style>
