<template>
	<view>
		
		<view class="foundDetail">
			<view class="video">
				<image src="../../static/images/find-the-details-img1.png" mode=""></image>
			</view>
		
			<view class="R-fixBtn fs12 white center">
				<view class="item">
					<view class="photo"><image src="../../static/images/find-the-details-img2.png" mode=""></image></view>
					<view>小萌姐</view>
				</view>
				<view class="item" @click="zan()">
					<view class="icon">
						<image v-show="!is_zan" src='../../static/images/find-the-details-icon2.png' mode=""></image>
						<image v-show="is_zan" src='../../static/images/find-the-details-icon2.1.png' mode=""></image>
					</view>
					<view>569</view>
				</view>
				<view class="item" @click="commentShow">
					<view class="icon"><image src="../../static/images/find-the-details-icon3.png" mode=""></image></view>
					<view>562</view>
				</view>
				<view class="item">
					<view class="icon"><image src="../../static/images/find-the-details-icon4.png" mode=""></image></view>
					<view>123</view>
				</view>
			</view>	
			
			<view class="describe white">荆防颗粒打落水狗挂号费了的水果还是光和热都扣了上复工姑父省了挂号费了缸发动机看深V和</view>
		</view>
		
		<view class="commentShow" v-show="is_commentShow">
			<view class="center pdb15">全部评论<span class="color9">235</span></view>
			<view class="closebtn"  @click="commentShow">×</view>
			<view class="detail_pjLis">
				<view class="item" v-for="(item,index) in messageData" :key="index">
					<view class="photo"><image src="../../static/images/details-img1.png" mode=""></image></view>
					<view class="cont">
						<view class="flexRowBetween pdt5 pdb10 fs13">
							<view class="name color6">哆啦A梦</view>
							<view class="time color9">2020-03-04</view>
						</view>
						<view class="text">根据瑞灵活赶紧来都搞好了电话沟通各环节的反馈给换了多个黑客帝国</view>
					</view>
				</view>
			</view>
			<view class="xqbotomBar pdlr4" style="height: 120rpx;">
				<view class="editInput pr">
					<image class="icon" src="../../static/images/details-icon9.png" mode=""></image>
					<input type="text" placeholder="我来说一说">
				</view>
				<view class="pubColor fs15 flexEnd">发送</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_show:false,
				is_zan:false,
				messageData:[{},{},{},{},{},{}],
				is_commentShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			zan(){
				const self = this;
				self.is_zan = !self.is_zan
			},
			commentShow(){
				const self = this;
				self.is_commentShow = !self.is_commentShow
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
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
	
</style>
