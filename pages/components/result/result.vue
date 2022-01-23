<template>
	<view class="bg-main">
		<view class="flex pt-4 justify-center align-center" style="flex-direction: column; height: 630.5rpx;">
			<image src="@/static/over.png" mode="scaleToFill" style="width: 696rpx; height: 590.5rpx;"></image>
		</view>
	<guidefairy></guidefairy>
		<view class="flex pl-5" style="margin-top: 80rpx;flex-direction: column; z-index: 999;" >
			<text class="text-white" style="font-size: 60rpx;">诊断结果</text>
			<text>
				\n经自助排查诊断，您的电脑故障为
			</text>
			<view class="mt-2 flex align-center justify-center bg-white round shadow" style="width: 317rpx; height: 150rpx;">
				<text class="text-lg text-red" style="font-weight: bold;">{{result_text}}</text>
			</view>
			<text class="mt-2 text-lg">请截图保存\n并将结果告诉维修人员</text>
			<text class="mt-2 text-lg">按下方按钮结束本向导</text>
		</view>
		<view class="mt-5 pl-5">
			<button class="cu-btn bg-blue round shadow flex align-center justify-center" style="width: 104rpx; height: 104rpx;" @click="cancel">
				<text class="iconfont icon-24gl-enter" style="font-size: 75rpx;"></text>
				</button>
		</view>
		<view style="padding-top: 99rpx;"></view>
		</view>
	</view>
</template>

<script>
	import guidefairy from '@/components/guidefairy.vue'
	export default {
		components:{
			guidefairy
		},
		data() {
			return {
				result:"",
				result_text:""
			}
		},
		onLoad() {
			uni.getStorage({
				key:'result_data',
				success: (res) => {
					console.log("读取向导结果成功")
					this.result = res.data
				},
				fail: ()=>{
					console.log("读取失败")
				}
			})
		},
		onReady(){
			this.judge(this.result)
		},
		methods: {
			judge(result){
				if(result == "C"){
					this.result_text = "系统故障或硬盘故障"
				}
				else if(result == "A"){
					this.result_text = "内存条或显卡故障"
				}
				else if(result == "B"){
					this.result_text = "主板或电源故障"
				}
				else if(result == "D"){
					this.result_text = "系统优化类故障"
				}
				else if(result == "E"){
					this.result_text = "其他类型问题"
				}
				else{
					this.result_text = "向导故障，请联系开发者"
				}
			},
			cancel(){
				// 直接返回首页
				uni.navigateBack({
					delta: 9
				})
			}
		}
	}
</script>

<style>

</style>
