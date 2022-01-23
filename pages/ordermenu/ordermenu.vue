<template>
	<view>
		<view class="bg-gradual-red text-center round text-xxl py-2 mt-3 px-2 mx-5">
			服务优品
		</view>
		<view>
		<shopitem :item="item" v-for="item of products" :key="item.id"></shopitem>
		</view>
	</view>
</template>

<script>
	import shopitem from '@/components/shopitem.vue'
	export default {
		components:{
			shopitem
		},
		onLoad() {
			uni.request({
				url:'http://localhost:80/api/shopitem.json',
				success: (res) => {
					this.products = res.data.shopitem
					console.log(this.products)
				}
			})
		},
		data() {
			return {
				products:[]
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
