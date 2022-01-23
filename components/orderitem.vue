<template>
	<view>
		<view class="flex bg-white mt-2" style="height:225rpx; width: 100%; flex-direction: row;">
			<image src="@/static/computer.png" mode="aspectFit" style="width: 196rpx; height: 196rpx;" class="ml-3 mt-2"></image>
			<view class="mt-3 ml-1 text-lg">
				{{!info.project ? info.title : info.project}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:['info'],
		name:"orderitem",
		data() {
			return {
				
			};
		}
	}
</script>

<style>

</style>
