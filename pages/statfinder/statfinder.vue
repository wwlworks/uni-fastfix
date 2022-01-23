<template>
	<view>
		<view class="cu-bar search bg-white">
				<view class="search-form round">
					<text class="cuIcon-search"></text>
					<input @focus="InputFocus" @blur="InputBlur" :adjust-position="false" type="text" placeholder="搜索寄修中的电脑..." confirm-type="search"></input>
				</view>
				<view class="action">
					<button class="cu-btn bg-green shadow-blur round">搜索</button>
				</view>
			</view>
		<fixitem :fitem="item" v-for="item of fixitems" :key="item.id"></fixitem>
	</view>
</template>

<script>
	import fixitem from '@/components/fixitem.vue'
	export default {
		components:{
			fixitem
		},
		data() {
			return {
				InputBottom : 0,
				// 维修标记 mark
				// mark = 1,待维修
				// mark = 2,维修中
				// mark = 3,维修完成
				fixitems:[
					{
						id:1,
						orderid:123456789,
						time:"2022年1月30日 0:00",
						status:"正在维修中",
						mark:2,
						CPU:"i7-7700k",
						GC:"GTX1650S",
						color:"白色"
					},{
						id:2,
						orderid:978654321,
						time:"2022年1月31日 0:00",
						status:"已送达维修点，待维修",
						mark:1,
						CPU:"i7k",
						GC:"GTX0S",
						color:"黑色"
					},{
						id:3,
						orderid:8426507193,
						time:"2022年1月32日 5:00",
						status:"已经维修完成，我们会尽快送还",
						mark:3,
						CPU:"i7-7700k",
						GC:"GTX1650S",
						color:"白色"
					}
				]
			}
		},
		methods: {
			InputFocus(e) {
				this.InputBottom = e.detail.height
			},
			InputBlur(e) {
				this.InputBottom = 0
			}
		}
	}
</script>

<style>

</style>
