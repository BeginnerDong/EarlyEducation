<template>
	<view>
		<view class="editMoney mglr4 radius10 whiteBj">
			<view class="card flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_myBankcard/user_myBankcard'}})">
				<view class="flex">到账银行卡</view>
				<view class="flexEnd">
					<view class="mgr10 fs13 color6" v-if="userInfoData.bank_no!=''">({{userInfoData.bank_no?userInfoData.bank_no:''}})</view>
					<view class="mgr10 fs13 color6" v-else>绑定银行卡</view>
					<view class="flexEnd">
						<image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image>
					</view>
				</view>
			</view>
			<view class="tixian">
				<view class="tit pdtb15">提现金额</view>
				<view class="input borderB1 mgb10 flex">
					<input type="number" v-model="submitData.count" />
				</view>
				<view class="flexRowBetween">
					<view class="fs12 color6">本次可提现金额{{userInfoData.balance}}元</view>
					<view class="fs13 red" @click="allCount">全部提现</view>
				</view>
				<view class="submitbtn">
					<button class="Wbtn" type="button" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提现</button>
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
				Utils:this.$Utils,
				mainData:{},
				userInfoData:{},
				submitData:{
					count:''
				}
			}
		},
		onLoad(options) {
			const self = this;
			
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			allCount(){
				const self = this;
				self.submitData.count = self.userInfoData.balance
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.userInfoData.bank==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请先绑定银行卡', 'none');
					return
				};
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
					
					const callback = (user, res) => {
						self.flowLogAdd();
					};
					self.$Utils.getAuthSetting(callback);	
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入提现金额', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				if(!wx.getStorageSync('user_info')||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:'提现',
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
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
						self.userInfoData.bank_no = self.userInfoData.bank_no.substr(self.userInfoData.bank_no.length-4)		
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},



		},
	};
</script>

<style>
	page{ background: #f5f5f5;}
	
	.editMoney{margin: 120rpx 4%;}
	.editMoney .card{padding: 30rpx 7%;background: #fafafa;}
	.editMoney .tixian{padding: 0rpx 7%;}
	.editMoney .input{line-height: 100rpx;}
	.editMoney .input input{ font-size: 60rpx; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box;}
	.editMoney .input::before{content: "￥"; font-size: 50rpx; margin-right: 10rpx;}
	.editMoney .submitbtn{padding:60rpx 0;}
</style> 
