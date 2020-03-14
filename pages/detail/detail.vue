<template>
	<view>
		
		<view class="detailxqBan pr">
			<view class="video"><image src="../../static/images/details-img.png" mode="widthFix"></image></view>
			
			<!-- 观看时长到期 -->
			<view class="timeEnd white center" v-show="!is_timeEnd">
				<view class="fs13">您的买卖南非观看时长已到期，如要继续观看本视频可将本视频海报进行分享后，获得免费观看一天时长，分享免费观看次数为两次</view>
				<view class="flexCenter mgt30">
					<view class="btn fs13" @click="Router.navigateTo({route:{path:'/pages/share/share'}})">去分享</view>
				</view>
				
			</view>
		</view>
		
		<view class="orderNav flexRowBetween pdb20 pdt5">
			<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')"><view class="tit">视频</view></view>
			<view class="tt flexCenter" :class="curr==2?'on':''"  @click="changeCurr('2')"><view class="tit">评论</view><span class="color9 fs12 mgl5 ">325</span></view>
		</view>
		
		<view v-show="curr==1">
			<view class="mglr4 xqInfor">
				<view class="pdb10 flexRowBetween">
					<view class="fs15 ftw" style="width: 70%;">经典中文儿歌</view>
					<view class="color9 fs12 flexEnd">时长：1:09:09</view>
				</view>
				<view class="cont fs13 color6">
					<view>管理客服电话还房贷两个号和个梵蒂冈悲愤交加鹤骨鸡肤供货方点击可供货方都拉黑干活的放假开个会过分的话看刚发的刚发的个人个人赛退热贴热推监管科了付赛的价格过节费考虑到加工费来电管家发了个结果翻到鬼斧神工金融机构日就是个交管理费见到过和公司就发了活过来了供货方的记录可挂号费了家里就结果回访供货方健康的两个号</view>
				</view>
			</view>
			
			<view class="xqbotomBar pdl15 pdr5 center" style="height: 120rpx;">
				<view class="payBtn" @click="Router.navigateTo({route:{path:'/pages/buyVIP/buyVIP'}})">立即购买</view>
				<view class="small-btn flexEnd fs12">
					<view class="child flexColumn">
						<view class="icon">
							<image src="../../static/images/details-icon4.png" mode=""></image>
						</view>
						<view>123</view>
					</view>
					<view class="child">
						<view class="icon">
							<image src="../../static/images/details-icon3.png" mode=""></image>
						</view>
						<view>569</view>
					</view>
					<view class="child">
						<view class="icon">
							<image src="../../static/images/details-icon5.png" mode=""></image>
						</view>
						<view>562</view>
					</view>
					<view class="child"  v-show="!is_expireShow" @click="expireShow">
						<view class="icon">
							<image src="../../static/images/details-icon6.png" mode=""></image>
						</view>
						<view>123</view>
					</view>
					<view class="child" v-show="is_expireShow" @click="Router.navigateTo({route:{path:'/pages/share/share'}})">
						<view class="icon">
							<image src="../../static/images/details-icon6.png" mode=""></image>
						</view>
						<view>123</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 评价 -->
		<view class="pdlr4"  v-show="curr==2">
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
				showView: false,
				wx_info:{},
				is_show:false,
				is_timeEnd:false,
				curr:1,
				messageData:[{},{},{},{}],
				is_expireShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	
</style>
