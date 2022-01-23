<template>
	<view style="width:100%">
		<view class="flex bg-gray align-center pl-3" style="height: 100rpx; font-weight: bold;">
			热点问题
		</view>
		<view class="cu-list menu margin-top card-menu" >
			<view class="cu-item arrow" @click="showhelp(1)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">如何完成自助排查向导？</text>
				</view>
			</view>
			<view class="cu-item arrow"  @click="showhelp(2)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">维修资讯怎么刷新？</text>
				</view>
			</view>
			<view class="cu-item arrow" @click="showhelp(3)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">下单了维修人员多久能上门？</text>
				</view>
			</view>
			<view class="cu-item arrow"   @click="showhelp(4)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">维修费用怎么给？</text>
				</view>
			</view>
			<view class="cu-item arrow"  @click="showhelp(5)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">修好后保修多久？</text>
				</view>
			</view>
			<view class="cu-item arrow"  @click="showhelp(6)">
				<view class="content">
					<text class="cuIcon-newfill text-blue"></text>
					<text class="text-grey">本系统的更新日志</text>
				</view>
			</view>
		</view>
		<wxy-popup v-model="show" :mode="mode" :height="height" :width="width" :border-radius="radius">
		    <view class="pop-container">
		        <view class="pop-item text-lg text-center text-red" style="font-weight: bold;"><text>帮助详情</text></view>
		        <view class="pop-item"><text>\n{{content}}</text></view>
		        <view class="pop-item text-center text-blue text-bold"><text>\n点击本弹窗外可关闭</text></view>
		    </view>
		</wxy-popup>
	</view>
</template>

<script>

	export default {
		components:{
	
		},
		data() {
			return {
				 show: false,
				 mode: 'center',
				 height: '',
				 width: '80%',
				 radius: '30',
				 content:'这是一段测试文字，具体逻辑明天写吧'
			}
		},
		methods: {
			showhelp(i){
				if(i==1){
					this.content = "请按照向导上的提示，结合自己实际情况作出选择，当选择结果生成后，请截图并把结果告诉预约上门的维修人员。\n 注意：本向导的结果数据将不会保存！"
					this.show = true
				}
				if( i == 2){
					this.content = "按右下角蓝色按钮即可刷新"
					this.show = true
				}
				if( i == 3){
					this.content = "下单后系统后台会在2小时内派单给维修人员，维修人员会在接单后1小时内上门维修，因此请耐心等待！"
					this.show = true
				}
				if(i == 4){
					this.content = "下单后即可通过微信支付支付维修费用！"
					this.show = true
				}
				if(i == 5){
					this.content = "保修三个月哦"
					this.show = true
				}
				if(i == 6){
					this.content = "2021年2月1日 V1.0Beta \n 初稿完成，还有很多功能需要完善！"
					this.show = true
				}
			}
		}
	}
</script>

<style>

</style>
