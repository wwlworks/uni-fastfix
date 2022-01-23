<template>
	<view>
		<view class="flex m-3" style="flex-direction: row;">
			<image src="@/static/computer.png" mode="aspectFit" style="width: 300rpx; height: 300rpx;"></image>
			<view class="flex text-lg text-bold m-2" style="flex-direction: column;line-height: 1.6;">
				<text>订单号：{{Fixitem.orderid}}</text>
				<text>CPU：{{Fixitem.CPU}}</text>
				<text>显卡：{{Fixitem.GC}}</text>
				<text>送达时间：\n {{Fixitem.time}}</text>
			</view>
		</view>
		<view class="flex mt-5 ml-3" style="flex-direction: column;">
			<text class="text-xxl text-blue text-shadow">时间线</text>
			<view class="cu-timeline">
				<view class="cu-time">{{Fixitem.time}}</view>
			<view class="cu-item cur cuIcon-noticefill">
				<view class="content bg-green shadow-blur">
					<text>{{Fixitem.time}}</text>
					<text>\n 目标已经送达维修点,即将开始维修</text>
				</view>
			</view>
			<view class="cu-item text-red cuIcon-repairfill" v-if="Fixitem.mark == 2 || Fixitem.mark == 3">
				<view class="content bg-red shadow-blur">
					目标电脑正在维修中，请耐心等待
				</view>
			</view>
			<view class="cu-item text-cyan cuIcon-squarecheckfill" v-if="Fixitem.mark == 3">
				<view class="content bg-cyan shadow-blur">
					目标电脑已经维修完成，我们将尽快送还
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
				Fixitem:{
					
				},
			}
		},
		onLoad(){
			uni.getStorage({
				key:'fixitem',
				success:(res) => {
					this.Fixitem = res.data
					uni.showToast({
						icon:"success",
						title:"加载成功",
						duration:700
					})
				},
				fail(error){
					console.log(error)
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>
	page{
		background-color: #fff;
	}
</style>
