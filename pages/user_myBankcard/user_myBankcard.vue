<template>
	<view>
		
		<view class="editLine pdlr4 fs13">
			<view class="item flexRowBetween">
				<view class="ll">开户人姓名</view>
				<view class="rr">
					<input type="text" v-model="submitData.bank_name" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户人电话</view>
				<view class="rr">
					<input type="text" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户行</view>
				<view class="rr">
					<input type="text" v-model="submitData.bank" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户行账号</view>
				<view class="rr">
					<input type="number" v-model="submitData.bank_no" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">验证码</view>
				<view class="rr flexEnd">
					<view style="width: 50%;">
						<input type="number" v-model="submitData.smsCode" placeholder="请输入" placeholder-class="placeholder" />
					</view>
					<view class="pubColor mgl10" @click="sendCode()" v-if="!hasSend">{{text}}</view>
					<view class="pubColor mgl10"  v-else>{{text}}</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100px;">
			<button class="btn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					bank:'',
					bank_name:'',
					phone:'',
					bank_no:'',
					smsCode:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone,
					
					}
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						console.log(user)
						self.userInfoUpdate();
					};
					self.$Utils.getAuthSetting(callback);	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {
					bank:self.submitData.bank,
					bank_name:self.submitData.bank_name,
					phone:self.submitData.phone,
					bank_no:self.submitData.bank_no,
				};
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.submitData.smsCode
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('绑定成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		},
	}
</script>


<style>
	@import "../../assets/style/editInfor.css";
	
	.editLine .item input{text-align: right;}
</style>

