<template>
	<view>
		
		<view class="foundList">
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
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
					user_no:uni.getStorageSync('user_info').user_no
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
			
		},
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	
	page{padding-bottom: 60rpx;background: #F5F5F5;}	
	
	.foundList .item{padding: 30rpx 4%;background: #fff;margin-bottom: 20rpx;}
	.foundList .item .video{width: 100%;height: 360rpx;border-radius: 10rpx;overflow: hidden;}
	.video image{width: 100%;height: 100%;}
	
	.foundList .photo{width: 60rpx;height: 60rpx;border-radius: 50%;margin-right: 10rpx;overflow: hidden;}
	.foundList .photo image{width: 100%; height: 100%;}
	
	.foundList .item .infor .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.sBtn{margin-left: 100rpx;}
	
</style>
