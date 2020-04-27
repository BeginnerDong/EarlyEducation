<template>
	<view>
		
		<view class="mglr4">
			<view class="vipCard center mgt15 fs15 radius10 oh">
				<view class="pic"><image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image></view>
				<!-- <view class="tit pdt15 pdb10">到期时间：2020-01-01至2020-12-30</view> -->
			</view>
		</view>
		
		<view class="xqbotomBar pdl15" style="height: 100rpx;">
			<view class="flex fs13">总计：<view class="price fs16 ftw">{{mainData.price}}</view></view>
			<button class="payBtn flexCenter" style="height: 100rpx;" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认订单</button>
		</view>
	
	
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				distriData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData','getDistriData'], self);
		},
		
		methods: {
			
			getDistriData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					child_no:uni.getStorageSync('user_info').user_no
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.distriData.push.apply(self.distriData,res.info.data)
					};
					self.$Utils.finishFunc('getDistriData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData = res.info.data[0]
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var orderList = [{
					product_id: self.mainData.id,
					count: 1,
					type: 1,
				}];
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				if(self.orderId){
					self.goPay()
					return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				
				const postData = {
					wxPay:{
						price:parseFloat(self.mainData.price)
					}
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				postData.payAfter = [];
				if(self.distriData.length>0){
					for (var i = 0; i < self.distriData.length; i++) {
						if(self.distriData[i].level==1){
							postData.payAfter.push(
								{
									tableName: 'FlowLog',
									FuncName: 'add',
									data: {
										count:((parseFloat(uni.getStorageSync('user_info').thirdApp.first_class))/100*parseFloat(self.mainData.price)).toFixed(2),
										thirdapp_id:2,
										status:1,
										trade_info:'推广佣金',
										type:2,
										account:1,
										behavior:1,
										user_no:self.distriData[i].parent_no,
										relation_user:uni.getStorageSync('user_info').user_no
									},
								},
							)
						}else if(self.distriData[i].level==2){
							postData.payAfter.push(
								{
									tableName: 'FlowLog',
									FuncName: 'add',
									data: {
										count:((parseFloat(uni.getStorageSync('user_info').thirdApp.first_class))/100*parseFloat(self.mainData.price)).toFixed(2),
										thirdapp_id:2,
										status:1,
										trade_info:'推广佣金',
										type:2,
										account:1,
										behavior:1,
										user_no:self.distriData[i].parent_no,
										relation_user:uni.getStorageSync('user_info').user_no
									},
								},
							)
						}
					}
				}
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										
										self.$Router.redirectTo({route:{path:'/pages/index/index'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								
								self.$Router.redirectTo({route:{path:'/pages/index/index'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.vipCard .pic{width: 100%;height: 433rpx;}
	.vipCard .pic image{width: 100%;height: 100%;}
	button {
		background: none;
		line-height: 1.5;
		margin: 0;
		border-radius: 0;
		font-size:15px
	}
	
	button::after {
		border: none;
	}
	
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
