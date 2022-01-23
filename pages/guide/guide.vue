<template>
	<view >
		<view class="flex pt-4 justify-center align-center" style="flex-direction: column; height: 630.5rpx;">
			<image src="@/static/step1.png" mode="scaleToFill" style="width: 696rpx; height: 590.5rpx;"></image>
		</view>
		<view class="flex" style="position: relative;">
			<image src="https://pic.imgdb.cn/item/61e77e762ab3f51d91c8c2f8.png" mode="scaleToFill" style="width: 452.5rpx; position: absolute; left: 350rpx; top:230rpx; z-index:6"></image>
		</view>
		<view class="flex pl-5" style="margin-top: 80rpx;flex-direction: column; z-index: 999;" >
		<text class="text-white" style="font-size: 60rpx;">欢迎！</text>
		<text>
			\n这个向导会帮助您排查常见的电脑故障
            从而帮助维修人员确定故障
            因此请认真按照本向导的指示进行选择
            准备好后就按下方的按钮开始吧！
        </text>
		</view>
		<view class="mt-5 pl-5 pt-5">
			<button class="cu-btn bg-blue round shadow flex align-center justify-center" style="width: 150rpx; height: 150rpx;" @click="start">
			<text class="iconfont icon-24gl-enter" style="font-size: 75rpx;"></text>
			</button>
		</view>
		<view style="padding-top: 225rpx;"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				choice:[],
				result:""
			}
		},
		methods: {
			start(){
				const found = this.choice.find(element => element == "01")
				console.log(found)
				if(found == "01"){
					uni.showToast({
					title: '请勿重复点击!',
					icon:'none',
					duration: 2000
					});
				}else{
					console.log(this.choice)
					this.choice.push("01")
					uni.setStorage({
						key:'choice_array',
						data:this.choice,
						success: () => {
							uni.showToast({
							title: '提交成功',
							icon:'none',
							duration: 2000
							});
						},
						fail: () => {
							uni.showToast({
							title: '提交失败',
							icon:'none',
							duration: 2000
							});
						}
					})
					uni.setStorage({
						key:'result_data',
						data:this.result,
						success(res) {
							console.log(res)
							console.log("向导结果初始化成功")
						},
						fail: () => {
							console.log("初始化失败")
						}
					})
				}
				uni.navigateTo({
				    url: './guide_01'
				});
			}
		}
	}
</script>

<style>

</style>
