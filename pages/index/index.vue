<template>
	<view>
		<view class="flex align-center justify-center" > 
			<image src="../../static/banner.png" mode="scaleToFill" style="height: 319.17rpx; width: 750rpx;"></image>
		</view>
		<view class="flex align-center justify-center" style="height: 145rpx; width: 750rpx;"  hover-class="bg-blue light" @click="gotoguide">
			<view class="cu-btn bg-blue round shadow lg" style="height: 94rpx; width: 690rpx;">
				自助排查故障
			</view>
		</view>
		<view class="flex" style="height: 335rpx; flex-direction: row; margin-top: 160rpx;">
			<view class="flex main-1 justify-center align-center" style="width:375rpx;" hover-class="bg-red light" @click="gotoorder">
			<view class="flex align-center justify-center" style="flex-direction: column;">
				<text class="iconfont icon-xiaoxi font-icon main-icon-1"></text>
				<text class="main-text text-white">预约维修</text>
			</view>
			</view>
			<view class="flex main-2 justify-center align-center" style="width:375rpx;" hover-class="bg-orange light" @click="gotonews">
			<view class="flex align-center justify-center" style="flex-direction: column;">
				<text class="iconfont icon-zixun font-icon main-icon-2"></text>
				<text class="main-text text-white">维修资讯</text>
			</view>
			</view>
		</view>
		<view class="flex" style="height: 335rpx; flex-direction: row;">
			<view class="flex main-3 justify-center align-center" style="width:375rpx;" hover-class="bg-green light" @click="gotofind">
			<view class="flex align-center justify-center" style="flex-direction: column;">
				<text class="iconfont icon-chaxun font-icon main-icon-3"></text>
				<text class="main-text text-white">状态查询</text>
			</view>
			</view>
			<view class="flex main-4 justify-center align-center" style="width:375rpx;" hover-class="bg-blue light" @click="gotosupport">
			<view class="flex align-center justify-center" style="flex-direction: column;">
				<text class="iconfont icon-bangzhu font-icon main-icon-4"></text>
				<text class="main-text text-white">帮助支持</text>
			</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {
			gotoguide(){
				uni.navigateTo({
					url:'../guide/guide',
					success: () => {
						uni.showToast({
						title: '正在进入向导',
						icon:'none',
						duration: 2000
						});
					}
				})
			},
			gotonews(){
				uni.navigateTo({
					url:'../newslist/newslist',
					success: () => {
					}
				})
			},
			gotosupport(){
				uni.navigateTo({
					url:'../support/support',
					success: () => {
					}
				})
			},
			gotoorder(){
				uni.navigateTo({
					url:'../ordermenu/ordermenu',
					success: () => {
					}
				})
			},
			gotofind(){
				uni.navigateTo({
					url:'../statfinder/statfinder',
					success: () => {
						
					}
				})
			}
		}
	}
</script>

<style>
	
</style>
