<template>
	<view>
		
		<view class="pdlr4 pdt15">
			<textarea v-model="submitData.description" placeholder="写标题并使用合适的话题" placeholder-class="placeholder" />
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdt15">
			<view class="flex videoList">
				<view class="item">
					<video :src="url" style="width: 100%;height:180rpx"></video>
				</view>
				<view class="item" v-if="submitData.mainImg&&submitData.mainImg.length==0"  @click="upLoadImg('mainImg')" style="background-color: #f5f5f5;color: #000000;text-align: center;line-height: 160rpx;font-size:80rpx">
					+
				</view>
				<view class="item" v-if="submitData.mainImg&&submitData.mainImg.length==0"   style="text-align: center;line-height: 160rpx">
					上传封面图
				</view>
				<view class="item" v-if="submitData.mainImg&&submitData.mainImg.length>0" >
					<image :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:''"></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">发布</button>
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
					type :1,
					description:'',
					mainImg:[],
					headImg:[],
					title:'',
					thirdapp_id:2,
					bannerImg:[]
				},
				url:''
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
			self.url = options.url;
			self.submitData.bannerImg = [{type:'vedio',url:self.url}]
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.headImg;
				delete newObject.title;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					const callback = (user, res) => {
						console.log(user)
						self.submitData.title = user.nickName;
						self.submitData.headImg = [{type:'image',url:user.avatarUrl}]
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'img'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.data.behavior = 0;
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('发布成功，等待后台审核', 'none', 1000)
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
				self.$apis.messageAdd(postData, callback);
			},
			
			previewImage: function(e) {
				var current = e.target.dataset.src
				uni.previewImage({
					current: current,
					urls: this.imageList
				})
			},
			
			
		}
	};
</script>

<style>
	textarea{padding: 0;height: 360rpx;}
	.videoList{flex-wrap: wrap;}
	.videoList .item{width: 150rpx;height: 180rpx;margin:0 30rpx 30rpx 0;}
	.videoList .item image{width: 100%;height: 100%;}
	.videoList .item:nth-of-type(4n){margin-right: 0;}
</style>
