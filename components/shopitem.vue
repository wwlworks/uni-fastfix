<template>
		<view class="bg-white m-2 shadow" style="width: 330rpx;flex-direction: column; float: left;" @click="gotoinfo">
			<image :src="item.img" mode="aspectFill" style="height: 350rpx;width: 295rpx;" class="mx-2 mt-2"></image>
			<view class="ml-2 text-black" style="font-size: 16px;">{{item.title}}</view>
			<view class="text-red ml-2 mt-1 mb-2">￥{{item.price}}</view>
		</view>
</template> 

<script>
	export default {
		props:['item'],
		name:"shopitem",
		data() {
			return {
				items : this.item
			};
		},
		methods:{
			gotoinfo(){
				uni.setStorage({
					key:'setitem',
					data:this.items,
					success: () => {
						console.log('product successful')
						uni.navigateTo({
							url:'../ordermenu/orderdetail/orderdetail',
							success: () => {
								console.log('jump successful')
							},
							fail: (error) => {
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
