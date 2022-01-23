<template>
	<view class="bg-info" style="height: 812rpx;">
		<view class="flex bg-white my-2 align-center" style="width: 100%; height: 100rpx;">
		<view class="text-lg pl-4">
			目前状态：<text class="text-orange">
				<text v-if="orders.flag === 1">待付款</text>
				<text v-else-if ="orders.flag === 2">订单正在进行中</text>
				<text v-else-if ="orders.flag === 3">订单已完成</text>
			</text>
		</view>
		</view>
		<view class="flex bg-white" style="height: 482rpx;flex-direction: column;">
			<view class="flex text-lg text-black align-center  pl-4" style="height: 71rpx;">
				订单编号 {{orders.code}}
			</view>
			<view class="s-divider"></view>
			<view class="flex text-lg text-black align-center  pl-4" style="height: 71rpx;">
				联系人 {{orders.contact}}
			</view>
			<view class="s-divider"></view>
			<view class="flex text-lg text-black align-center  pl-4" style="height: 71rpx;">
				预约地址 {{orders.place}}
			</view>
			<view class="s-divider"></view>
			<view class="flex text-lg text-black align-center  pl-4" style="height: 71rpx;">
				下单时间 {{orders.time}}
			</view>
			<view class="s-divider"></view>
			<view class="flex text-lg text-black align-center  pl-4" style="height: 71rpx;">
				<view style="white-space:pre-wrap" class="pt-2">服务说明：
				{{orders.memo}}
				</view>
			</view>
		</view>
		<orderitem v-bind:info="orders"></orderitem>
		<view class="flex align-end pr-2" style="width: 100%; height: 104rpx; flex-direction: row-reverse;">
			<button class="cu-btn bg-green round mx-2" v-if="orders.flag == 2" @click="hurry">催一下</button>
			<button class="cu-btn bg-orange round mx-2" v-else-if="orders.flag == 1" @click="pay">去付款</button>
			<button class="cu-btn bg-red round ml-2" v-if="orders.flag !=3">取消</button>
			<button class="cu-btn bg-blue round ml-2" @click="ref">返回</button>
		</view>
	</view>
</template>

<script>
	import orderitem from '@/components/orderitem.vue'
	export default {
		components:{
			orderitem
		},
		data() {1
			return {
				orders:{
				},
				status:""
			}
		},
		onLoad(){
			uni.getStorage({
				key:'order_item',
				success:(res)=>{
					this.orders = res.data
					console.log("读取订单数据成功")
				}
			})
		},
		methods: {
			ref(){
				uni.navigateBack({
				    delta: 1
				});
			},
			hurry(){
				console.log("在催了在催了.jpg")
			},
			pay(){
				console.log("就假设已经给了钱吧")
			}
		}
	}
</script>

<style>

</style>
