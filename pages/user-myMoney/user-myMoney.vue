<template>
	<view>
		
		<view class="myMoney pdlr4 pubBj white center">
			<view class="bigNum ftw">{{userInfoData.totalCount?userInfoData.totalCount.count:''}}</view>
			<view class="fs154 mgt10">总佣金</view>
			<view class="fs13" style="margin-top: 40rpx;">已提现金额：￥{{userInfoData.hasWithdraw?userInfoData.hasWithdraw.count:''}}</view>
		</view>
		
		<view class="mglr4 pdtb15 flexRowBetween">
			<view class="flex">
				<view class="mgr15">可提现金额</view>
				<view>{{userInfoData.balance?userInfoData.balance:''}}元</view>
			</view>
			<view class="txBtn" @click="Router.navigateTo({route:{path:'/pages/user-cashOut/user-cashOut'}})">提现</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="orderNav flexRowBetween pdt5 pdb15 borderB1">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')">推广佣金</view>
			<view class="tt flexCenter" :class="curr==2?'on':''"  @click="changeCurr('2')">浏览量收益</view>
		</view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll">
					<view class="">{{item.trade_info}}</view>
					<view class="fs13 color9">{{item.create_time}}</view>
				</view>
				<view class="rr red">{{item.count}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				curr:1,
				tuiguangData:[{},{},{},{}],
				browseData:[{},{},{},{}],
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					behavior:1,
					type:2,
					count:['>',0]
				},
				userInfoData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					self.searchItem.behavior = self.curr;
					self.getMainData(true)
				}
			},
			
			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getAfter = {
					hasWithdraw: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							type:2,
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2,withdraw:1
						    }
						  ]
						},
					},
					totalCount: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							type:2,
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2,count:['>',0]
						    }
						  ]
						},
					},
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.getMainData()
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.messageData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.flowLogGet(postData, callback);
			},

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
