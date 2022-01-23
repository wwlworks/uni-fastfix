<template>
	<view class="flex mx-2 bg-white rounded align-center my-2" style="width: 700rpx; height: 252rpx;position: relative; flex-direction: row; ">
		<image src="@/static/computer.png" mode="aspectFit" style="width: 196rpx; height: 196rpx;" class="ml-3"></image>
		<view class="mx-2 flex" style="flex-direction: column;width: 452rpx;">
			<view class="text-gray-lg text-normal">
				<text>下单时间: {{item.time}}</text>
				<br />
				<text>预约地点: {{item.place}} </text>
				<br />
				<text>维修人员: {{item.worker}} </text>
				<br />
				<text>维修项目: {{item.project}}</text>
				<br />
			</view>
			<view class="mt-1 flex" style="flex-direction: row-reverse;">
				<button v-show="item.flag === 2" class="cu-btn bg-green round sm ml-2 shadow" @click="hurry">{{text}}</button>
				<button class="cu-btn bg-grey light round sm ml-2 shadow" @click="jump">详细信息</button>
				<button v-show="item.flag != 3" class="cu-btn bg-red round sm ml-2 shadow" @click="cancel">取消预约</button>
			</view>
			
		</view>
		<text class="text-sm text-orange" style="position:absolute; top:17rpx; right: 45rpx;">{{item.status}}</text>
	</view>
</template>

<script>
	export default{
		// 创建功能配置
		data() {
			return {
				text:"催一下",
				orders:this.item
			}
		},
		props:['item'],
		methods:{
			hurry(){
				console.log("在催了在催了.jpg")
			},
			cancel(){
				console.log("取消成功，感谢您的使用")
			},
			jump(){
				// 将订单信息存储到本地缓存中供订单详情读取
				uni.setStorage({
					key:'order_item',
					data:this.orders,
					success:()=>{
						console.log('订单信息存入成功')
					}
				})
				uni.navigateTo({
				    url: '../components/info/info'
				});
			}
		},
	}
</script>

<style>
</style>
