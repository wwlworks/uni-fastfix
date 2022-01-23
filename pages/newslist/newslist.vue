<template>
	<view>
			<view>
				<newscard v-bind:item="item" v-for="item of news" :key="item.id"></newscard>
			</view>
			<view class="flex">
				<button 
				class="cu-btn bg-blue round shadow" 
				style="width: 104rpx; height: 104rpx; position:fixed; right: 70rpx; top:1200rpx;"
				@tap="LoadProgress"
				>
					<view class="cuIcon-loading2" style="font-size: 40rpx;"></view>
				</button>
			</view>
			<view class="load-progress" :class="loadProgress!=0?'show':'hide'" :style="[{top:CustomBar+'px'}]">
				<view class="load-progress-bar bg-green" :style="[{transform: 'translate3d(-' + (100-loadProgress) + '%, 0px, 0px)'}]"></view>
				<view class="load-progress-spinner text-green"></view>
			</view>
	</view>
</template>

<script>
	import newscard from '@/components/newscard.vue'
	export default {
		components:{
			newscard
		},
		data() {
			return {
				newsRaw:[],
				news:[],
				loadProgress: 0
			}
		},
		onLoad() {
			this.getNews()
		},
		methods: {
			getNews(){
				uni.request({
					url:'http://localhost:80/api/newslist.json',
					success: (res) => {
						this.newsRaw = res.data.newslist
						this.news = this.newsRaw.reverse()
						uni.showToast({
							title:'加载成功',
							duration:1000,
							fail(error) {
								console.log(error)
							}
						})
					}
				})
			},
			LoadProgress(e) {
				this.loadProgress = this.loadProgress + 3;
				if (this.loadProgress < 100) {
					setTimeout(() => {
					this.LoadProgress();
					}, 100)
				} else {
					this.getNews()
					this.loadProgress = 0;
				}
			}
		}
	}
</script>

<style>
</style>
