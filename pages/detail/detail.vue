<template>
	<view>
		
		<view class="detailxqBan pr">
			<view class="video" style="position: relative;">
				<cover-view style="position: absolute;top: 0;z-index: 1;height: 450rpx;" v-show="!play&&mainData.bannerImg[0]">
					<cover-view style="width: 100%;height: 100%" v-show="!play">
						<cover-image v-show="!play" style="width: 100%;height: 100%;" :src="mainData&&mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></cover-image>
					</cover-view>
				</cover-view>
				<cover-view style="position: absolute;top: 38%;z-index: 1;left:46%" @click="vedioPlay" v-show="!play&&mainData.bannerImg[0]">
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
				<button class="payBtn"  v-if="bannerData&&bannerData[0]&&bannerData[0].url!='1'"  style="font-size: 14px;" open-type="getUserInfo" @getuserinfo="getAuth">立即购买</button>
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
					
					<view style="font-size: 24rpx;" class="child" @tap="shareEvn">
						<view class="icon">
							<image src="../../static/images/details-icon6.png" mode=""></image>
						</view>
						<view>{{mainData.share_count?mainData.share_count:'0'}}</view>
					</view>
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
			
			<view class="xqbotomBar pdlr4" style="height: 120rpx;" v-if="bannerData&&bannerData[0]&&bannerData[0].url!='1'">
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
		<hchPoster ref="hchPoster" :canvasFlag.sync="canvasFlag" @cancel="canvasCancel" :posterObj.sync="posterData" />
		<view :hidden="canvasFlag" @click="canvasCancel">
			<!-- 海报 要放外面放组件里面 会找不到 canvas-->
			<canvas class="canvas" canvas-id="myCanvas"></canvas><!-- 海报 -->
		</view>
		
		<view class="share-pro" v-if="deliveryFlag">
			<view class="share-pro-mask"></view>
			<view class="share-pro-dialog" :class="deliveryFlag?'open':'close'">
				<view class="close-btn" @tap="closeShareEvn">
					<span class="font_family">&#xe6e6;</span>
				</view>
				<view class="share-pro-title">分享</view>
				<view class="share-pro-body">
					<view class="share-item">
						<button open-type="share">
							<view class="font_family share-icon">&#xe6fa;</view>
							<view>分享给好友</view>
						</button>
					</view>
					<view class="share-item" @tap="createCanvasImageEvn">
						<view class="font_family share-icon">&#xe6f9;</view>
						<view>生成分享图片</view>
					</view>
				</view>
		
			</view>
		</view>
	</view>
</template>

<script>
	import hchPoster from '../../wxcomponents/hch-poster/hch-poster.vue'
	export default {
		components: {
			hchPoster,
		},
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
				},
				bannerData:[],
				canvasFlag: true,
				deliveryFlag: false,
				posterData: {}
			}
		},
		
		onLoad(options) {
			const self = this;
			
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			console.log('啊啊啊啊',options)
			if(options.user_no){
				self.id = options.id;
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData','getBannerData','getQrData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:options.user_no
				})
			}else if(options.scene){
				 var scene = decodeURIComponent(options.scene);
				 self.id = scene.split("-")[0];
				 var parent_no = scene.split('-')[1];
				 console.log('scene',scene)
				 console.log('self.id',self.id)
				 console.log('parent_no',parent_no)
				 const callback = (res) => {
				 	self.$Utils.loadAll(['getUserInfoData','getBannerData','getQrData'], self);
				 };
				 self.$Token.getProjectToken(callback, {
				 	refreshToken: true,parent_no:parent_no
				 })
			}else{
				self.id = options.id;
				const callback = (res) => {
					self.$Utils.loadAll(['getUserInfoData','getBannerData','getQrData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true
				})
			}
			
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if(new Date(new Date().toLocaleDateString()).getTime()/1000==self.userInfoData.end_time&&self.userInfoData.behavior<2){
				self.userInfoUpdate('oneDay')
			};
			self.shareCount()
			if (ops.from === 'button') {
				
				return {
					title: '妈宝育儿-'+self.mainData.title,
					path: '/pages/detail/detail?user_no='+uni.getStorageSync('user_info').user_no+'&id='+self.id, //点击分享的图片进到哪一个页面
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
			}else{
				return {
					title: '妈宝育儿-'+self.mainData.title,
					path: '/pages/detail/detail?user_no='+uni.getStorageSync('user_info').user_no+'&id='+self.id, //点击分享的图片进到哪一个页面
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
			
			
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: self.id+'-'+uni.getStorageSync('user_info').user_no,
					page: 'pages/detail/detail',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
					self.$Utils.finishFunc('getQrData');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			createCanvasImageEvn() {
				const self = this;
				console.log(self.QrData.info.url)
				console.log(self.mainData.mainImg[0].url)
				Object.assign(this.posterData, {
					 url: self.mainData.mainImg[0].url, //商品主图
					//icon: 'https://img0.zuipin.cn/mp_zuipin/poster/hch-hyj.png', //醉品价图标
					title: self.mainData.title, //标题
					//discountPrice: self.mainData.price, //折后价格
					//orignPrice: "300.00", //原价
					code: self.QrData.info.url, //小程序码
					closeBtn:'../../static/images/find-the-details-icon.png'
				})
				this.$forceUpdate(); //强制渲染数据
				setTimeout(() => {
					this.canvasFlag = false; //显示canvas海报
					this.deliveryFlag = false; //关闭分享弹窗
					this.$refs.hchPoster.createCanvasImage(); //调用子组件的方法
				}, 1000)
			},
			
			// 分享弹窗
			shareEvn() {
				this.deliveryFlag = true;
				console.log(this.deliveryFlag)
			},
			// 关闭分享弹窗
			closeShareEvn() {
				this.deliveryFlag = false;
			},
			// 取消海报
			canvasCancel(val) {
				this.canvasFlag = val;
			},
			
			getBannerData() {
				const self = this;
				self.bannerData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					city_id:self.city_id
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', ['轮播图']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.bannerData.push.apply(self.bannerData, res.info.data)
					}
					console.log('self.bannerData', self.bannerData)
					self.$Utils.finishFunc('getBannerData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getAuth(){
				const self = this;
				const callback = (user, res) => {
					self.Router.navigateTo({route:{path:'/pages/buyVIP/buyVIP'}})
				};
				self.$Utils.getAuthSetting(callback);
			},
			
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
			
			shareCount() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.mainData.id
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {	
						self.mainData.share_count = self.mainData.share_count + 1
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.shareCount(postData, callback);
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

<style lang="scss">
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom:140rpx;}
	
	.orderNav .tt{line-height: 72rpx;}
	.orderNav .tt.on::after{width: 0;}
	.orderNav .tt.on .tit{position: relative;}
	.orderNav .tt.on .tit::after{content: ""; width:50rpx; height: 6rpx;background: #42d040; position: absolute; bottom: 0;left: 50%;transform: translateX(-50%);border-radius: 5rpx;}
	
	.xqbotomBar{z-index: 0;}
	
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
	@font-face {
		font-family: 'font_family';
		/* project id 1065286 */
		src: url('//at.alicdn.com/t/font_1065286_3bsye5aijur.eot');
		src: url('//at.alicdn.com/t/font_1065286_3bsye5aijur.eot?#iefix') format('embedded-opentype'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.woff2') format('woff2'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.woff') format('woff'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.ttf') format('truetype'),
			url('//at.alicdn.com/t/font_1065286_3bsye5aijur.svg#font_family') format('svg');
	}
	
	.font_family {
		font-family: "font_family" !important;
		font-size: 16px;
		font-style: normal;
		-webkit-font-smoothing: antialiased;
		-webkit-text-stroke-width: 0.2px;
		-moz-osx-font-smoothing: grayscale;
	}
	
	/* 按钮去掉边框 */
	button::after {
		border: none;
	}
	
	button {
		margin-left: 0;
		margin-right: 0;
		padding-left: 0;
		padding-right: 0;
		color: #1c1c1c;
		font-size: 28rpx;
		background: none;
	}
	
	.button-hover {
		color: #1c1c1c;
		background: none;
	}
	
	/*每个页面公共css */
	.content {
		text-align: center;
		height: 100%;
	}
	
	/* .share-btn {
		padding: 30upx 60upx;background-color: $uni-btn-color;color: $uni-text-color-inverse;
	} */
	
	.share-pro {
		display: flex;
		align-items: center;
		justify-content: flex-end;
		flex-direction: column;
		z-index: 5;
		line-height: 1;
		box-sizing: border-box;
	
		.share-pro-mask {
			width: 100%;
			height: 100%;
			position: fixed;
			top: 0;
			right: 0;
			bottom: 0;
			left: 0;
			background: rgba(0, 0, 0, 0.5);
			z-index: 2;
		}
	
		.share-pro-dialog {
			width: 750rpx;
			height: 310rpx;
			overflow: hidden;
			background-color: #fff;
			border-radius: 24rpx 24rpx 0px 0px;
			position: relative;
			box-sizing: border-box;
			position: fixed;
			bottom: 0;
			z-index: 3;
			.close-btn {
				padding: 20rpx 15rpx;
				position: absolute;
				top: 0rpx;
				right: 29rpx;
			}
	
			.share-pro-title {
				font-size: 28rpx;
				color: #1c1c1c;
				padding: 28rpx 41rpx;
				background-color: #f7f7f7;
			}
	
			.share-pro-body {
				display: flex;
				flex-direction: row;
				justify-content: space-around;
				font-size: 28rpx;
				color: #1c1c1c;
	
				.share-item {
					display: flex;
					flex-direction: column;
					justify-content: center;
					justify-content: space-around;
	
					.share-icon {
						text-align: center;
						font-size: 70rpx;
						margin-top: 39rpx;
						margin-bottom: 16rpx;
						color: #42ae3c;
					}
	
					&:nth-child(2) {
						.share-icon {
							color: #ff5f33;
						}
					}
				}
			}
		}
	
		/* 显示或关闭内容时动画 */
	
		.open {
			transition: all 0.3s ease-out;
			transform: translateY(0);
		}
	
		.close {
			transition: all 0.3s ease-out;
			transform: translateY(310rpx);
		}
	
	}
	
	.canvas {
		position: fixed !important;
		top: 0 !important;
		left: 0 !important;
		display: block !important;
		width: 100% !important;
		height: 100% !important;
		z-index: 10;
	}
</style>
