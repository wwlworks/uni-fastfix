<template>
	<view>
		<guidebg></guidebg>
		<guidefairy></guidefairy>
		<view class="flex pl-5" style="margin-top: 80rpx;flex-direction: column; z-index: 999;" >
			<text class="text-white" style="font-size: 60rpx;">Step 3</text>
			<text>
				\n
				现在，请仔细检查系统中的各项功能是否正常
				\n
				<text class="text-xl">现在请判断：</text>
				\n
				<text style="font-weight: bold;">您的电脑是否是否有死机、卡机、\n上不了网等影响使用类问题？</text>
			</text>
			<view class="mt-5 pt-3 flex" style="flex-direction: row;">
				<button class="cu-btn bg-blue round shadow flex align-center justify-center" style="width: 130rpx; height: 130rpx;" @click="yes">
				<text class="text-xl">是</text>
				</button>
				<button class="cu-btn bg-blue round shadow flex align-center justify-center ml-5" style="width: 130rpx; height: 130rpx;" @click="no">
				<text class="text-xl">否</text>
				</button>
			</view>
			<view style="padding-top: 240rpx;"></view>
			</view>
		</view>
	</view>
</template>

<script>
	import guidebg from '@/components/guidebg.vue'
	import guidefairy from '@/components/guidefairy.vue'
	export default {
		components:{
			guidebg,
			guidefairy
		},
		onLoad() {
			uni.getStorage({
				key:'choice_array',
				success: (res) => {
					console.log('读取选择分支成功 by step3')
					this.choice = res.data
				}
			})
			uni.getStorage({
				key:'result_data',
				success: (res) => {
					console.log('读取向导结果成功 by step3')
					this.result = res.data
				}
			})
		},
		data() {
			return {
				choice:[],
				result:""
			}
		},
		methods: {
			yes(){
						const found = this.choice.find(element => element == "31" || element == "32")
						if(found == "31" || found == "32"  ){
							uni.showToast({
							title: '请勿重复选择!',
							icon:'none',
							duration: 2000
							});
						}
						else{
							this.choice.push("31")
							this.result = "D"
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
								success: () => {
									console.log("向导结束，存储结果数据成功")
								},
								fail: () => {
									console.log("存储失败")
								}
							})
						}
						uni.navigateTo({
							url:'../components/result/result',
							success: () => {
								uni.showToast({
								title: '向导结束，正在生成结果',
								icon:'none',
								duration: 2000
								});
							}
						})
					},
					no(){
						const found = this.choice.find(element => element == "31" || element == "32")
						if(found == "31" || found == "32"  ){
							uni.showToast({
							title: '请勿重复选择!',
							icon:'none',
							duration: 2000
							});
						}
						else{
							this.choice.push("32")
							this.result = "E"
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
								success: () => {
									console.log("向导结束，存储结果数据成功")
								},
								fail: () => {
									console.log("存储失败")
								}
							})
						}
						uni.navigateTo({
							url:'../components/result/result',
							success: () => {
								uni.showToast({
								title: '向导结束，正在生成结果',
								icon:'none',
								duration: 2000
								});
							}
						})
					}
				}
			}
</script>

<style>

</style>
