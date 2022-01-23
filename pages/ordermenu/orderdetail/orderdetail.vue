<template>
	<view>
		<view class="flex">
			<image :src="item.img" mode="aspectfill" style="width: 100%; height:700rpx;"></image>
		</view>
		<view class="flex mt-2 ml-3" style="flex-direction: column;">
			<view class="text-red">
				<text class="text-xxl text-price"></text>
				<text class="text-sl">{{item.price}}</text>
			</view>
			<view class="mt-2 text-black text-xl text-bold">{{item.title}}</view>
			<view class="mt-2 text-black text-normal">
				<text>{{item.des}}</text>
			</view>
			<view class="mt-2 mb-2 bg-cyan mr-3 rounded shadow-blur" style="height: 330rpx;">
			<view class="p-2">
				项目说明：
				<text >
					{{item.des}}
					注意：本项目一次只能订购一次，如还需要重复订购，请重新下单\n下单后请在信息填写页填写相关预约信息，您的电话号码将是我们确认订单的唯一凭证！
				</text>
			</view>
			</view>
		</view>
		<view style="position:fixed; bottom:0;width: 100%; height: 120rpx;">
			<view class="cu-bar bg-white tabbar border shop shadow">
				<button class="action">
					<view class="cuIcon-info text-green">
					</view>
					帮助
				</button>
				<view class="action">
					<view class=" cuIcon-shop"></view> 返回
				</view>
				<view class="action">
					<view class="cuIcon-service">
					</view>
					客服
				</view>
				<view class="btn-group">
					<button class="cu-btn bg-red round shadow-blur" @click="create">立即预约</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				item:[]
			}
		},
		onLoad(){
			uni.getStorage({
				key:'setitem',
				success: (res) => {
					console.log('set')
					this.item = res.data
				}
			})
		},
		methods: {
			create(){
				uni.navigateTo({
					url:'../createinfo/createinfo',
					success: () => {
						uni.showToast({
							icon:"success",
							title:'请填写预约信息'
						})
					}
				})
			}
		}
	}
</script>

<style>
	page{
		background-color: #fff;
	}
</style>
