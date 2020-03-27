<template>
	<view>
		
		<view class="detailxqBan pr">
			<view class="video" style="position: relative;">
				<cover-view style="position: absolute;top: 0;z-index: 998;height: 450rpx;" v-show="!play&&mainData.bannerImg[0]">
					<cover-view style="width: 100%;height: 100%" v-show="!play">
						<cover-image v-show="!play" style="width: 100%;height: 100%;" :src="mainData&&mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></cover-image>
					</cover-view>
				</cover-view>
				<cover-view style="position: absolute;top: 38%;z-index: 999;left:46%" @click="vedioPlay" v-show="!play&&mainData.bannerImg[0]">
					<cover-view v-show="!play" style="width: 100%;height: 100%">
						
						<cover-image v-show="!play" style="width: 80rpx;height: 80rpx" src="../../static/images/details-icon8.png"></cover-image>
					</cover-view>
				</cover-view>
				<video id="myVideo" v-if="mainData.bannerImg[0]" style="width: 100%;" auto-pause-if-navigate="true" page-gesture="true" :src="mainData&&mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''"></video>
			</view>
			
			<!-- 观看时长到期 -->
			<!-- <view class="timeEnd white center" v-show="!is_timeEnd">
				<view class="fs13">您的买卖南非观看时长已到期，如要继续观看本视频可将本视频海报进行分享后，获得免费观看一天时长，分享免费观看次数为两次</view>
				<view class="flexCenter mgt30">
					<view class="btn fs13" @click="Router.navigateTo({route:{path:'/pages/share/share'}})">去分享</view>
				</view>
				
			</view> -->
		</view>
		
		<view class="orderNav flexRowBetween pdb20 pdt5">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')"><view class="tit">视频</view></view>
			<view class="tt flexCenter" :class="curr==2?'on':''"  @click="changeCurr('2')"><view class="tit">评论</view><span class="color9 fs12 mgl5 ">{{mainData.comment&&mainData.comment.count?mainData.comment.count:'0'}}</span></view>
		</view>
		
		<view v-show="curr==1">
			<view class="mglr4 xqInfor">
				<view class="pdb10 flexRowBetween">
					<view class="fs15 ftw" style="width: 70%;">{{mainData.title?mainData.title:''}}</view>
					<view class="color9 fs12 flexEnd">时长：{{mainData.keywords?mainData.keywords:''}}</view>
				</view>
				<view class="cont fs13 color6">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
				</view>
			</view>
			
			<view class="xqbotomBar pdl15 pdr5 center" style="height: 120rpx;">
				<view class="payBtn" @click="Router.navigateTo({route:{path:'/pages/buyVIP/buyVIP'}})">立即购买</view>
				<view class="small-btn flexEnd fs12">
					<view class="child flexColumn">
						<view class="icon">
							<image src="../../static/images/details-icon4.png" mode=""></image>
						</view>
						<view>{{mainData.view_count?mainData.view_count:'0'}}</view>
					</view>
					<view class="child" @click="Utils.stopMultiClick(clickGood)">
						<view class="icon">
							<image :src="mainData.goodMe&&mainData.goodMe.length>0?'../../static/images/details-icon3.png':'../../static/images/found-icon3.png'" mode=""></image>
						</view>
						<view>{{mainData.good&&mainData.good.count?mainData.good.count:'0'}}</view>
					</view>
					<view class="child">
						<view class="icon">
							<image src="../../static/images/details-icon5.png" mode=""></image>
						</view>
						<view>{{mainData.comment&&mainData.comment.count?mainData.comment.count:'0'}}</view>
					</view>
					
					<button style="font-size: 24rpx;" class="child" open-type="share">
						<view class="icon">
							<image src="../../static/images/details-icon6.png" mode=""></image>
						</view>
						<view>{{mainData.share_count?mainData.share_count:'0'}}</view>
					</button>
				</view>
			</view>
		</view>
		
		<!-- 评价 -->
		<view class="pdlr4"  v-show="curr==2" scroll-y="true">
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
					<input type="text" v-model="submitData.description" placeholder="我来说一说">
				</view>
				<button class="pubColor fs15 flexEnd" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">发送</button>
			</view>
		</view>
		
		<!-- 过期弹框 -->
		<view class="" v-show="is_expireShow">
			<view class="black-bj"></view>
			<view class="ExpireShow">
				<view class="text fs13 center white">您的免费观看时长已到期，如要继续观看本视频可将本视频海报进行分享后，获得免费观看一天时长，分享免费观看次数为两次</view>
				<view class="bigArrow">
					<image src="../../static/images/details-icon7.png" mode=""></image>
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
				showView: false,
				wx_info:{},
				is_show:false,
				is_timeEnd:false,
				curr:1,
				messageData:[],
				is_expireShow:false,
				mainData:{},
				play:false,
				userInfoData:{},
				submitData:{
					title:'',
					description:'',
					mainImg:[],
					thirdapp_id:2,
					type:2,
					relation_id:'',
					relation_table:'Article'
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.user_no){
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:options.user_no
				})
			}else{
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true
				})
			}
			
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '亲子早教-'+self.mainData.title,
					path: '/pages/detail/detail?user_no='+self.user_no+'&id='+self.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData.mainImg[0].url,
					success: function(res) {
						// 转发成功
						if(new Date(new Date().toLocaleDateString()).getTime()/1000==self.userInfoData.end_time&&self.userInfoData.behavior<2){
							self.userInfoUpdate('oneDay')
						}
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title: '亲子早教-'+self.mainData.title,
					path: '/pages/detail/detail?user_no='+self.user_no+'&id='+self.id, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData.mainImg[0].url,
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}
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
					relation_table:'Article',
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
			
			getUserInfoData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id: 2,
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.getMainData()
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			vedioPlay() {
				const self = this;
				var videoContextPrev = wx.createVideoContext('myVideo')
				if(self.play){
					videoContextPrev.pause();
					self.play = false
				}else{
					if(self.userInfoData.end_time!=0&&self.userInfoData.end_time==new Date(new Date().toLocaleDateString()).getTime()/1000&&self.userInfoData.behavior<2){
						self.expireShow();
						return
					};
					videoContextPrev.play();
					self.play = true;
					if(self.userInfoData.end_time==0){
						self.userInfoUpdate()
					}
				}
			},
			
			userInfoUpdate(type) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = {
					end_time:new Date(new Date().toLocaleDateString()).getTime()/1000+3*86400
				};
				if(type&&type=='oneDay'){
					postData.data.end_time = self.userInfoData.end_time + 86400
					postData.data.behavior = self.userInfoData.behavior + 1
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			expireShow(){
				const self = this;
				self.is_expireShow = !self.is_expireShow
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				postData.getAfter = {
					goodMe: {
						token:uni.getStorageSync('user_token'),
						tableName: 'Log',
						searchItem: {
							status:['in',[1,-1]],
							type:1,
							user_no:uni.getStorageSync('user_info').user_no,
							relation_table:'Article'
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
							relation_table:'Article'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:1,relation_table:'Article'
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
							relation_table:'Article'
						},
						middleKey: 'id',
						key: 'relation_id',
						condition: 'in',
						compute:{
						  count:[
						    'count',
						    'count',
						    {
						      status:1,type:2,relation_table:'Article'
						    }
						  ]
						},
					},
					
				};
				postData.saveFunction = [{
					FuncName: 'viewArticle',
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
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			
			getMessageData(isNew) {
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					type:2,
					relation_id:self.id,
					relation_table:'Article'
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.orderNav .tt{line-height: 72rpx;}
	.orderNav .tt.on::after{width: 0;}
	.orderNav .tt.on .tit{position: relative;}
	.orderNav .tt.on .tit::after{content: ""; width:50rpx; height: 6rpx;background: #42d040; position: absolute; bottom: 0;left: 50%;transform: translateX(-50%);border-radius: 5rpx;}
	
	.xqbotomBar{z-index: 45;}
	
	.timeEnd{position: absolute;top: 0;right: 0;bottom: 0;left: 0;background: rgba(0,0,0,0.5);box-sizing: border-box;padding:90rpx 9% 30rpx 9%;line-height: 48rpx;}
	.timeEnd .btn{width: 142rpx;height: 50rpx;line-height: 50rpx;text-align: center;border-radius: 36rpx;color: #fff;background-color: #42D040;box-shadow: 0 4rpx 0 #1cb71a;}
	
	.ExpireShow{position: fixed;top:480rpx;left:4%;right: 4%;bottom:150rpx;z-index: 55;}
	.ExpireShow .text{width: 85%;margin: 0 auto;line-height: 50rpx;}
	.ExpireShow .bigArrow{width: 365rpx;height: 350rpx;position: absolute;right: 40rpx;bottom:0 ;}
	.ExpireShow .bigArrow image{width: 100%;height: 100%;}
	button {
		background: none;
		line-height: 1.5;
		margin: 0;
		border-radius: 0;
		
	}
	
	button::after {
		border: none;
	}
	
	.button-hover {
		color: #000000;
		background: none;
	}
</style>
