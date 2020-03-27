<template>
	<view>
		
		<view class="foundDetail">
			<view class="video">
				<video autoplay="true" controls="false" v-if="mainData.bannerImg[0]" style="width: 100%;" auto-pause-if-navigate="true" page-gesture="true" :src="mainData&&mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''"></video>
			</view>
		
			<view class="R-fixBtn fs12 white center">
				<view class="item">
					<view class="photo" style="overflow: hidden;border-radius: 50%;"><image :src="mainData.headImg&&mainData.headImg[0]?mainData.headImg[0].url:''" mode=""></image></view>
					<view>{{mainData.title}}</view>
				</view>
				<view class="item" @click="Utils.stopMultiClick(clickGood)">
					<view class="icon">
				
						<image :src="mainData.goodMe&&mainData.goodMe.length>0?'../../static/images/find-the-details-icon2.png':'../../static/images/find-the-details-icon2.1.png'" mode=""></image>
					</view>
					<view>{{mainData.good&&mainData.good.count?mainData.good.count:'0'}}</view>
				</view>
				<view class="item" @click="commentShow">
					<view class="icon"><image src="../../static/images/find-the-details-icon3.png" mode=""></image></view>
					<view>{{mainData.comment&&mainData.comment.count?mainData.comment.count:'0'}}</view>
				</view>
				<view class="item">
					<view class="icon"><image src="../../static/images/find-the-details-icon4.png" mode=""></image></view>
					<view>{{mainData.view_count?mainData.view_count:'0'}}</view>
				</view>
			</view>	
			
			<view class="describe white">{{mainData.description?mainData.description:''}}</view>
		</view>
		
		<view class="commentShow" v-show="is_commentShow">
			<view class="center pdb15">全部评论<span class="color9">{{mainData.comment&&mainData.comment.count?mainData.comment.count:'0'}}</span></view>
			<view class="closebtn"  @click="commentShow">×</view>
			<view class="detail_pjLis">
				<view class="item" v-for="(item,index) in messageData" :key="index">
					<view class="photo"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="cont">
						<view class="flexRowBetween pdt5 pdb10 fs13">
							<view class="name color6">{{item.title}}</view>
							<view class="time color9">{{item.create_time}}</view>
						</view>
						<view class="text">{{item.description}}</view>
					</view>
				</view>
			</view>
			<view class="xqbotomBar pdlr4" style="height: 120rpx;">
				<view class="editInput pr">
					<image class="icon" src="../../static/images/details-icon9.png" mode=""></image>
					<input type="text" placeholder="我来说一说" v-model="submitData.description"  >
				</view>
				<button class="pubColor fs15 flexEnd" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">发送</button>
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
				is_zan:false,
				messageData:[
					
				],
				is_commentShow:false,
				mainData:{},
				submitData:{
					title:'',
					description:'',
					mainImg:[],
					thirdapp_id:2,
					type:2,
					relation_id:'',
					relation_table:'Message'
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			clickGood() {
				const self = this;
				uni.setStorageSync('canClick', false);	
				if (self.mainData.goodMe.length == 0) {
					self.addGoodLog()
				} else {
					self.updateGoodLog()
				};
			},
			
			addGoodLog() {
				const self = this;
				const postData = {};
				postData.data = {
					type: 1,
					title: '点赞成功',
					relation_id: self.mainData.id,
					relation_table:'Message',
					user_no: uni.getStorageSync('user_info').user_no,
					thirdapp_id:2
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData.goodMe.push({
							status: 1,
							id: res.info.id
						});
						self.mainData.good.count = self.mainData.good.count+1
					} else {
						self.$Utils.showToast('点赞失败', 'none', 1000)
					};
					uni.setStorageSync('canClick', true);	
				};
				self.$apis.logAdd(postData, callback);
			},
			
			
			updateGoodLog() {
				const self = this;
			
				const postData = {
					searchItem: {
						id: self.mainData.goodMe[0].id
						
					},
					data: {
						status: -self.mainData.goodMe[0].status
					}
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res.solely_code == 100000) {
						self.mainData.goodMe[0].status = -self.mainData.goodMe[0].status;
						if(self.mainData.goodMe[0].status==1){
							self.mainData.good.count = self.mainData.good.count+1
			
						}else{
							self.mainData.good.count = self.mainData.good.count-1
							self.$Utils.showToast('取消成功', 'none', 1000)
						}
					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					};
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.description==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('评论不能为空', 'none')
					return
				};
				const callback = (user, res) => {
					console.log(user)
					self.submitData.title = user.nickName;
					self.submitData.mainImg = [{type:'image',url:user.avatarUrl}]
					self.messageAdd();
				};
				self.$Utils.getAuthSetting(callback);	
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {	
						self.submitData.description = '';
						self.mainData.comment.count = self.mainData.comment.count + 1
						self.getMessageData(true)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			
			zan(){
				const self = this;
				self.is_zan = !self.is_zan
			},
			
			commentShow(){
				const self = this;
				self.is_commentShow = !self.is_commentShow
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				//postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
					type:1,
					id:self.id
					//behavior:1
					//user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					goodMe: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:1,
							user_no:uni.getStorageSync('user_info').user_no,
							relation_table:'Message'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
					},
					good: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						searchItem: {
							status:1,
							type:1,
							relation_table:'Message'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:1,relation_table:'Message'
						    }
						  ]
						},
					},
					comment: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Message',
						searchItem: {
							status:1,
							type:2,
							relation_table:'Message'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2,relation_table:'Message'
						    }
						  ]
						},
					},
					
				};
				postData.saveFunction = [{
					FuncName: 'viewMessage',
					searchItem:{
						id:self.id
					}
				}];
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.relation_id = self.mainData.id;
						self.getMessageData()
					};
					
				};
				self.$apis.messageGet(postData, callback);
			},
			
			getMessageData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:2,
					relation_id:self.id,
					relation_table:'Message'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.messageData.push.apply(self.messageData, res.info.data);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	.foundDetail{position: fixed;height: 100%;box-sizing: border-box;top: 0;right: 0;bottom: 0;left: 0;background: #222;}
	
	.foundDetail .video{width: 100%;height: 420rpx;position: absolute;top:28%;left: 0;right: 0;overflow: hidden;z-index: 1;}
	.foundDetail .video image{width: 100%;height: 100%;}
	.describe{position: absolute;bottom: 6%;left: 4%;right: 4%;}
	
	.R-fixBtn{position: fixed;bottom: 20%;right: 24rpx;z-index:10;}
	.R-fixBtn .item{margin-bottom: 36rpx;}
	.R-fixBtn image{width: 100%;height: 100%;}
	.R-fixBtn .photo{width: 80rpx;height: 80rpx;border-radius: 50%;margin: 0 auto;}
	.R-fixBtn .icon{width: 36rpx;height: 36rpx;margin: 0 auto;}
	
	
	/* 评论弹框 */
	.commentShow{background: #fff;height: 75%;border-radius: 10rpx 10rpx 0 0;position: fixed;bottom: 0;left: 0;right: 0;box-sizing: border-box;padding: 30rpx 4% 130rpx 4%; z-index: 99;}
	.commentShow .detail_pjLis{height:90%;overflow-y: auto;}
	button {
		background: none;
		line-height: 1.5;
	}
	
	button::after {
		border: none;
	}
	
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
