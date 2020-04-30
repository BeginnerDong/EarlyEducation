<template>
	<view>
		
		<view class="proList flexRowBetween mglr4">
			<view class="item boxShaow whiteBj" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/foundDetail/foundDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="video">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					<view class="time flex"><image class="sIcon" src="../../static/images/home-icon5.png" mode=""></image>{{item.keywords}}</view>
				</view>
				<view class="infor">
					<view class="flexRowBetween pdb10">
						<view class="flex fs12 color6">
							<view class="photo"><image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:''" mode=""></image></view>
							<view>{{item.title}}</view>
						</view>
						<!-- <view class="R-dian"><image src="../../static/images/found-icon5.png" mode=""></image></view> -->
					</view>
					<view class="tit avoidOverflow2 fs13">{{item.description}}</view>
				</view>
			</view>
		</view>
		
		<!-- <view class="foundList">
			<view class="item" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/foundDetail/foundDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="flexRowBetween fs13">
					<view class="flex pdb10">
						<view class="photo">
							<image :src="item.headImg&&item.headImg[0]?item.headImg[0].url:''" mode=""></image>
						</view>
						<view>{{item.title}}</view>
					</view>
					<view class="color9">{{item.create_time}}</view>
				</view>
				<view class="video" style="position: relative;">
					<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
					<image src="../../static/images/release-icon3.png" style="width: 80rpx;height: 80rpx;position: absolute;top: 38%;left:44%;"></image>
				</view>
				<view class="infor center flexEnd fs12 pdt15">
					<view class="sBtn flex">
						<view><image class="icon" src="../../static/images/found-icon1.png" mode=""></image></view>
						<view class="">{{item.view_count?item.view_count:'0'}}</view>
					</view>
					<view class="sBtn flex">
						<view><image class="icon" src="../../static/images/found-icon2.png" mode=""></image></view>
						<view class="">{{item.comment&&item.comment.count?item.comment.count:'0'}}</view>
					</view>
					<view class="sBtn flex">
						<view><image class="icon" :src="item.goodMe&&item.goodMe.length>0?'../../static/images/found-icon4.png':'../../static/images/found-icon3.png'" mode=""></image></view>
						<view class="">{{item.good&&item.good.count?item.good.count:'0'}}</view>
					</view>
				</view>
			</view>
		</view> -->
		
		<view class="R-fixIcon" @click="upLoadVideo"><image src="../../static/images/found-icon.png" mode=""></image></view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/found/found'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">发现</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				proData:[{},{},{},{}],
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			upLoadVideo(type) {
				const self = this;	
				if(self.userInfoData.is_member==0){
					uni.showModal({
						title: '提示',
						content: '会员用户才可发布视频，是否立即购买',
						showCancel: true,
						cancelText: '暂不',
						confirmText: '去购买',
						success: res => {
							if (res.confirm) {
								self.Router.navigateTo({route:{path:'/pages/buyVIP/buyVIP'}})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						},
					});
					return
				};
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.Router.navigateTo({route:{path:'/pages/found-fabu/found-fabu?url='+res.info.url}})
						/* self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						self.userInfoUpdate() */
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseVideo({
					count: 1,
					sourceType: ['camera', 'album'],
					maxDuration:15,
					
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePath;
						var file = res.tempFilePath;
						var obj = res.tempFilePath.lastIndexOf(".");
						var ext = res.tempFilePath.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:res.size,start:0,chunkSize:res.size,originName:'video'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
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
				//postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					type:1,
					behavior:1
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
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
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
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}	
	
	
	.foundList .item{padding: 30rpx 4%;background: #fff;margin-bottom:20rpx;}
	.foundList .item .video{width: 100%;height: 360rpx;border-radius: 10rpx;overflow: hidden;}
	.video image{width: 100%;height: 100%;}
	
	.foundList .photo{width: 60rpx;height: 60rpx;border-radius: 50%;margin-right: 10rpx;overflow: hidden;}
	.foundList .photo image{width: 100%; height: 100%;}
	
	.foundList .item .infor .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.sBtn{margin-left: 100rpx;}
	
	.R-fixIcon{width: 90rpx;height: 90rpx;position: fixed;top: 48%;right: 30rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
	
	.proList .item{height: auto;}
	.proList .item .infor .photo{width: 44rpx;height: 44rpx;margin-right: 20rpx;border-radius: 50%;overflow: hidden;}
	.proList .item .infor image{width: 100%;height: 100%;display: block;}
	.R-dian{width: 48rpx;height: 8rpx;}
	.proList .item .infor .tit{line-height: 38rpx;}
</style>
