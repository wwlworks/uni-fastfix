<template>
	<view>
		<view class="solids-bottom flex justify-center align-center text-xl bg-gradual-blue" style="height:100rpx;">
			<text >正在填写预约信息</text>
		</view>
		<form @submit="formSubmit">
			<view class="cu-form-group margin-top">
				<view class="title">*联系人姓名</view>
				<input placeholder="作为维修人员确认身份的凭据" name="contact"></input>
			</view>
			<view class="cu-form-group">
				<view class="title">*手机号码</view>
				<input placeholder="请输入您的手机号码" name="phone"></input>
				<view class="cu-capsule radius">
					<view class='cu-tag bg-blue '>
						+86
					</view>
					<view class="cu-tag line-blue">
						中国大陆
					</view>
				</view>
			</view>
			<view class="cu-form-group">
				<view class="title">*地点</view>
				<input placeholder="格式: XX栋+房间号 例:15栋703" name="address"></input>
				<text class='cuIcon-locationfill text-orange'></text>
			</view>
			<view class="cu-form-group">
				<view class="title">*上门时间点选择（点击时间选择）</view>
				<picker mode="time" :value="time" start="00:00" end="23:59" @change="TimeChange">
					<view class="picker">
						{{time}}
					</view>
				</picker>
			</view>
			<view class="cu-form-group">
				<view class="title">*上门日期选择（点击日期选择）</view>
				<picker mode="date" :value="date" start="2015-09-01" end="2025-09-01" @change="DateChange">
					<view class="picker">
						{{date}}
					</view>
				</picker>
			</view>
			<view class="cu-form-group align-start">
				<view class="title">故障简要描述</view>
				<textarea maxlength="-1" :disabled="modalName!=null" @input="textareaBInput" placeholder="包括:什么时候发生的 持续多久 怎么发生的故障 或 具体对服务提出什么要求"></textarea>
			</view>
			<view class="cu-bar bg-white margin-top">
				<view class="action">
					故障图片上传
				</view>
				<view class="action">
					{{imgList.length}}/4
				</view>
			</view>
			<view class="cu-form-group">
				<view class="grid col-4 grid-square flex-sub">
					<view class="bg-img" v-for="(item,index) in imgList" :key="index" @tap="ViewImage" :data-url="imgList[index]">
					 <image :src="imgList[index]" mode="aspectFill"></image>
						<view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
							<text class='cuIcon-close'></text>
						</view>
					</view>
					<view class="solids" @tap="ChooseImage" v-if="imgList.length<4">
						<text class='cuIcon-cameraadd'></text>
					</view>
				</view>
			</view>
			<view class="cu-form-group">
				<view class="title">备注</view>
				<input placeholder="对预约维修项目的备注" name="memo"></input>
			</view>
			<view class="f-divider"></view>
			<view class="solids-bottom flex pl-2 align-center bg-gradual-pink" style="height:70rpx;">
				<text>将要预约的维修项目</text>
			</view>
			<orderitem :info = "item"></orderitem>
			<view class="f-divider"></view>
			<view class="flex justify-center align-center p-4">
				<button class='cu-btn bg-green shadow ' style="width: 250rpx;height: 80rpx; font-size: 20px;" form-type="submit">提交预约</button>
				</view>
		</form>
	</view>
</template>

<script>
	import orderitem from '@/components/orderitem.vue'
	export default {
		components:{
			orderitem
		},
		data() {
			return {
				item:[],
				time: '20:00',
				date: '2022-1-22',
				imgList: [],
				modalName: null,
				textareaBValue: '',
			}
		},
		onLoad() {
			uni.getStorage({
				key:'setitem',
				success: (res) => {
					console.log('set')
					this.item = res.data
				}
			})
		},
		methods: {
			TimeChange(e) {
				this.time = e.detail.value
			},
			DateChange(e) {
				this.date = e.detail.value
			},
			textareaBInput(e) {
				this.textareaBValue = e.detail.value
			},
			ChooseImage() {
				uni.chooseImage({
					count: 4, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						if (this.imgList.length != 0) {
							this.imgList = this.imgList.concat(res.tempFilePaths)
						} else {
							this.imgList = res.tempFilePaths
						}
					}
				});
			},
			ViewImage(e) {
				uni.previewImage({
					urls: this.imgList,
					current: e.currentTarget.dataset.url
				});
			},
			DelImg(e) {
				uni.showModal({
					title: '警告',
					content: '确定要删除这张照片吗？',
					cancelText: '再想想',
					confirmText: '确定',
					success: res => {
						if (res.confirm) {
							this.imgList.splice(e.currentTarget.dataset.index, 1)
						}
					}
				})
			},
			formSubmit: function(e) {
			    var formdata = e.detail.value
				formdata.desc = this.textareaBValue
				formdata.time = this.time
				formdata.date = this.date
				formdata.img  = this.imgList
				formdata.itemName = this.item.title
				if(formdata.contact == "" || formdata.phone == "" || formdata.address== "" || formdata.time == "" || formdata.date == ""){
					 uni.showModal({
					      content: '星号项未填写完毕！',
					      showCancel: true
					 });
				}else{
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
	page{
		background-color: #fff;
	}
</style>
