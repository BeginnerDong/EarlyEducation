<template>
	<view>
		
		<view class="">
			<view class="banner-box pdlr4">
				<view class="banner">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#ff5e43">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item"  @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj mglr4 pdtb15 fs12">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon.png"></image>
				<view class="tit">亲子</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">早教</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">动画片</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">国学</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/productList/productList'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">英语</view>
			</view>
		</view>
		
		<view class="pdlr4">
			<view class="fs15 pdt5 ftw">推荐课程</view>
			<view class="proList flexRowBetween">
				<view class="item boxShaow" v-for="(item,index) in proData" :key="index" @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
					<view class="video">
						<image src="../../static/images/home-img.png" mode=""></image>
						<view class="time flex"><image class="sIcon" src="../../static/images/home-icon5.png" mode=""></image>03:09</view>
					</view>
					<view class="infor center">
						<view class="tit avoidOverflow">经典中文儿歌</view>
						<view class="text fs10 color9 avoidOverflow">最受欢迎的而她给你歌曲</view>
					</view>
				</view>
			</view>
			
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/found/found'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">发现</view>
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
				is_show: false,
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				proData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;}	
	
	.swiper-box {height: 300rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image {width: 100%;height: 100%;box-sizing: border-box;border-radius: 10rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 20%;text-align: center;color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:60rpx;height:60rpx; margin: 0 auto 10rpx auto; }
	
	
</style>
