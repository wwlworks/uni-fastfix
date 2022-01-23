<template>
	<view >
	<guidebg></guidebg>
	<guidefairy></guidefairy>
		<view class="flex pl-5" style="margin-top: 80rpx;flex-direction: column; z-index: 999;" >
		<text class="text-white" style="font-size: 60rpx;">Step 1</text>
		<text>
			\n
			现在，在电脑关机的情况下，请尝试按下您电脑的开机键，并等待<text class="text-red" style="font-weight: bold;">\t10\t</text>秒
			\n
			<text class="text-xl">现在请观察：</text>
			\n
			<text style="font-weight: bold;">您的电脑是否能正常启动？\n 是否有显示画面？</text>
		</text>
		<view class="mt-5 pt-3 flex" style="flex-direction: row;">
			<button class="cu-btn bg-blue round shadow flex align-center justify-center" style="width: 130rpx; height: 130rpx;" @click="yes">
			<text class="text-xl">是</text>
			</button>
			<button class="cu-btn bg-blue round shadow flex align-center justify-center ml-5" style="width: 130rpx; height: 130rpx;" @click="no">
			<text class="text-xl">否</text>
			</button>
		</view>
		<view style="padding-top: 202rpx;"></view>
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
		data() {
			return {
				choice:[]
			}
		},
		onLoad() {
			uni.getStorage({
				key:'choice_array',
				success: (res) => {
					console.log('读取选择分支成功')
					this.choice = res.data
				}
			})
		},
		methods: {
			yes(){
				const found = this.choice.find(element => element == "11" || element == "12")
				if(found == "11" || found == "12"  ){
					uni.showToast({
					title: '请勿重复选择!',
					icon:'none',
					duration: 2000
					});
				}
				else{
					this.choice.push("11")
					uni.setStorage({
						key:'choice_array',
						data:this.choice,
						success: () => {
							uni.showToast({
							title: '提交成功!',
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
				}
				uni.navigateTo({
				    url: './guide_11'
				});
			},
			no(){
				const found = this.choice.find(element => element == "11" || element == "12")
				if(found == "11" || found == "12"  ){
					uni.showToast({
					title: '请勿重复选择!',
					icon:'none',
					duration: 2000
					});
				}
				else{
					this.choice.push("12")
					uni.setStorage({
						key:'choice_array',
						data:this.choice,
						success: () => {
							uni.showToast({
							title: '提交成功!',
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
					uni.navigateTo({
					    url: './guide_12'
					});
				}
			}
		}
	}
</script>

<style>

</style>
