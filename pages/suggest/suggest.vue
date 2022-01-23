<template>
	<view>
		<form @submit="formsubmit">
			<view class="cu-form-group margin-top">
				<view class="title">昵称</view>
				<input placeholder="请输入昵称" name="name"></input>
			</view>
			<view class="cu-form-group">
				<view class="title">联系方式</view>
				<input placeholder="请留下您的联系方式" name="contact"></input>
			</view>
			<view class="cu-form-group">
				<textarea maxlength="-1" :disabled="modalName!=null" @input="textareaAInput" placeholder="请输入投诉的内容或对系统开发的建议~"></textarea>
			</view>
			<view style="height: 200rpx;" class="flex align-center justify-center pt-5">
				<button class="cu-btn bg-blue round" style="width: 200rpx;height: 200rpx;" form-type="submit">提交</button>
			</view>
		</form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				textareaAValue: ''
			}
		},
		methods: {
			textareaAInput(e) {
				this.textareaAValue = e.detail.value
			},
			formsubmit(e){
				 var suggestdata = e.detail.value
				 suggestdata.content = this.textareaAValue
				 console.log(suggestdata)
				 if(suggestdata){
					 uni.showModal({
					      content: '由于技术限制，假设数据已经上传完毕,即将返回首页',
					      showCancel: true,
					 	 success: (res) => {
					 	 	if(res.confirm){
					 			uni.navigateBack({
					 				delta:999
					 			})
					 		}
					 	 }
					 });
				 }
			}
		}
	}
</script>

<style>

</style>
