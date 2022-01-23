<template>
	<view>
		<view class="flex bg-white" style="height: 341rpx; flex-direction: row;">
			<view class="flex justify-center align-center" style="width: 40%;">
				<image  src="@/static/head.png" mode="aspectFit" style="height: 160rpx; width: 160rpx;"></image>
			</view>
			<view class="flex align-center justify-start" >
				<text class="text-black text-xl">ID:{{id}}\n
				<text class="text-sm">所属单位：\n{{address}}</text>
				</text>
			</view>
		</view>
		<view class="mt-2 bg-white flex align-center" style="height: 115rpx; flex-direction: row;" @click="gotoaddress">
			<view class="iconfont icon-dizhi ml-5 text-green" style="font-size:30px;">
			</view>
			<view class="text-xl ml-4 text-black">
				我的常用地址
			</view>
			<view class="iconfont icon-jinru-copy text-black mr-2" style="position: absolute; right: 0;"></view>
		</view>
		<view class="mt-2 bg-white flex align-center" style="height: 115rpx; flex-direction: row;" @click="gotosuggest">
			<view class="iconfont icon-xianshi_jinggao ml-5 text-red" style="font-size:30px;">
			</view>
			<view class="text-xl ml-4 text-black">
				投诉建议
			</view>
			<view class="iconfont icon-jinru-copy text-black mr-2" style="position: absolute; right: 0;"></view>
		</view>
		<view class="bg-white flex align-center" style="height: 115rpx; flex-direction: row;" @click="gotomyinfo">
			<view class="iconfont icon-diannao-copy ml-5 text-blue" style="font-size:25px;">
			</view>
			<view class="text-xl ml-4 text-black">
			个人信息
			</view>
				<view class="iconfont icon-jinru-copy text-black mr-2" style="position: absolute; right: 0; "></view>
		</view>
		<view class="bg-white flex align-center" style="height: 115rpx; flex-direction: row;" @click="gotoabout">
			<view class="iconfont icon-qunzuduoren ml-5 text-red" style="font-size:25px;">
			</view>
			<view class="text-xl ml-4 text-black">
			关于我们
			</view>
				<view class="iconfont icon-jinru-copy text-black mr-2" style="position: absolute; right: 0;"></view>
		</view>
		<view class="flex justify-center align-center mt-2" style="flex-direction: column;">
			<button class="cu-btn bg-red lg" style="width: 690rpx;" @click="logout">退出账号</button>
			<view style="height:348rpx;"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:"wwlworks",
				address:"广东省 广州市 华南农业大学珠江学院"
			}
		},
		methods: {
			logout(){
				console.log("已经退出登陆啦")
			},
			gotoaddress(){
				uni.navigateTo({
					url:'addressmod/addressmod'
				})
			},
			gotosuggest(){
				uni.navigateTo({
					url:"../suggest/suggest"
				})
			},
			gotomyinfo(){
				uni.navigateTo({
					url:"myinfo/myinfo"
				})
			},
			gotoabout(){
				uni.navigateTo({
					url:'../about/about'
				})
			}
		}
	}
</script>

<style>

</style>
