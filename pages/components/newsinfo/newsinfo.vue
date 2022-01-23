<template>
	<view>
		<view class="mt-4 ml-3" style="font-size: 50rpx;">
			{{news.title}}
		</view>
		<view class="mt-3 ml-3 text-lg text-gray" >
			{{news.author}} <text class="ml-2">发布时间：{{news.time}}</text>
		</view>
	    <u-parse :content="article" @preview="preview" @navigate="navigate" class="mt-2"></u-parse>
	</view>
</template>

<script>
	import uParse from '@/components/u-parse/u-parse.vue'
	export default {
		components: {
		    uParse
		  },
		data() {
			return {
				news:[],
				article:'我还啥都没写呢'
			}
		},
		onLoad() {
			uni.getStorage({
				key:'news_item',
				success:(res)=>{
					this.news = res.data
				}
			})
		},
		methods: {
			preview(src, e) {
			      // do something
			},
			navigate(href, e) {
			      // do something
			}
		},
	}
</script>

<style>
@import url("@/components/u-parse/u-parse.css");
page{
	background-color: #fff;
}
</style>
