<template>
	<view class="m-2 p-4 rounded bg-white flex" style="flex-direction: column; " @click="gotofixinfo">
		<view class="solid-bottom pb-2">
			<text class="text-gray">订单号：{{fitem.orderid}}</text>
			<text style="float:right" class="text-orange">
				<text v-if="fitem.mark === 1">待维修</text>
				<text v-else-if ="fitem.mark === 2">维修中</text>
				<text v-else-if ="fitem.mark === 3">维修完成</text>
			</text>
		</view>
		<view class="flex mt-2" style="flex-direction: row;">
			<image src="@/static/computer.png" mode="aspectFit" style="height: 204rpx; width: 204rpx;"></image>
			<view class="flex pl-3 text-lg" style="flex-direction: column;">
				<text>CPU: {{fitem.CPU}}</text>
				<text>显卡: {{fitem.GC}}</text>
				<text>送达时间: \n{{fitem.time}}</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"fixitem",
		props:['fitem'],
		data() {
			return {
				fitems : this.fitem
			};
		},
		methods:{
			gotofixinfo(){
				uni.setStorage({
					key:"fixitem",
					data:this.fitems,
					success() {
						console.log('fixitem set')
						uni.navigateTo({
							url:'../components/fixiteminfo/fixiteminfo',
							fail(error) {
								console.log(error)
							}
						})
					}
				})
			}
		}
	}
</script>

<style>

</style>
