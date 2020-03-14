<template>
	<view>
		
		<view class="editLine pdlr4 fs13">
			<view class="item flexRowBetween">
				<view class="ll">开户人姓名</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户人电话</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户行</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">开户行账号</view>
				<view class="rr">
					<input type="number" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">验证码</view>
				<view class="rr flexEnd">
					<view style="width: 50%;">
						<input type="number" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
					<view class="pubColor mgl10">获取验证码</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100px;">
			<button class="btn">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res){
				    console.log('getMainData', res);
				    self.mainData.push.apply(self.mainData,res.info.data);		        
				};
				self.$apis.orderGet(postData, callback);
			}
		},
	}
</script>


<style>
	@import "../../assets/style/editInfor.css";
	
	.editLine .item input{text-align: right;}
</style>

