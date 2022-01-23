<template>
		<view class="cu-card article">
			<view class="cu-item shadow">
				<view class="title"  @tap="jumptoinfo">
					<view class="text-cut">{{item.title}}</view>
				</view>
				<view class="content">
					<image :src="item.img"
					 mode="aspectFill"></image>
					<view class="desc">
						<view class="text-content">{{item.brief}}</view>
						<view>
							<view class="cu-tag bg-green light sm round">{{item.tag}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
</template>

<script>
	export default {
		props:['item'],
		name:"newscard",
		data() {
			return {
				news:this.item
			};
		},
		methods:{
			jumptoinfo(){
				uni.setStorage({
					key:'news_item',
					data:this.news,
					success:()=>{
					},
					fail() {
						console.log("error")
					}
				})
				uni.navigateTo({
				    url: '../components/newsinfo/newsinfo',
					success: () => {
						console.log("success")
					},
					fail(error) {
						console.log(error)
					}
				});
			}
		}
	}
</script>

<style>

</style>
