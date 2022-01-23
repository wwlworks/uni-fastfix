<template>
	<view>
		<view class="bg-gradual-orange flex align-center" style="height: 80rpx;">
			<text class="text-white ml-4 text-lg">常用地址</text>
			<text class="text-white cuIcon-add  mr-4" style="position: absolute; right: 0; font-size: 20px;"></text>
		</view>
		<view class="bg-white m-3 rounded shadow pt-2 pl-2" style="height: 200rpx;">
			<text class="text-xl text-bold text-black">{{contact}}</text>，
			<text class="text-lg text-bold text-black">{{phone}}</text>
			<text class="text-xxl">\n\n {{address}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				contact:"1",
				phone:"2",
				address:"3"
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
