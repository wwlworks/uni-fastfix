<template>
	<view>
		<view class="flex align-center fixed-top justify-center" style="height: 85rpx; background-color: #EEEEF4; ">
			<view class="mx-s my-s bg-white round" style="height: 56rpx; width: 714rpx;">
				<view class="flex align-center pl-2" style="flex-direction: row;height: 56rpx;">
					<text class="iconfont icon-sousuo" style="font-size: 23px;"></text>
					<text class="pl-1">搜索订单</text>
				</view>
			</view>
		</view>
		<view class="flex" style="flex-direction: column;">
			<view class="tab_nav">
				<view class="navTitle" v-for="(item,index) in navList" :key="index" >
					<view :class="{'active':isActive === index}" @click="checked(index)">
						{{item.title}}
					</view>
				</view>
			</view>
			<view class="nav_item" v-if="isActive==0">
				<view class="flex py-1" style="flex-direction: column;">
				<ordercard  v-bind:item="item" v-for="item of items" :completed="completed" :key="item.id"></ordercard>
				</view>
			</view>
			<view class="nav_item" v-if="isActive==1">
				<view class="flex py-1" style="flex-direction: column;">
				<ordercard  v-bind:item="item" v-for="item of items" v-if="item.flag === 1" :key="item.id"></ordercard>
				</view>
			</view>
			<view class="nav_item" v-if="isActive==2">
				<ordercard  v-bind:item="item" v-for="item of items" v-if="item.flag === 2" :key="item.id"></ordercard>
			</view>
			<view class="nav_item" v-if="isActive==3">
				<ordercard  v-bind:item="item" v-for="item of items" v-if="item.flag === 3" :key="item.id"></ordercard>
			</view>
		</view>
	</view>
</template>

<script>
import ordercard from '@/components/ordercard.vue'
export default {
	components:{
		ordercard
	},
	data() {
		return {
			TabCur: 0,
			scrollLeft: 0,
			completed:true,
			items:[],
			isActive: 0,
			navList:[
			{
				index: 0,
				title: '全部'
			},{
				index: 1,
				title: "待付款"
			},{
				index: 2,
				title: "进行中"
			},{
				index:3,
				title:"已完成"
			}
			]
		};
	},
	onLoad(){
		// 调用api获取数据
		this.getOrders()
	},
	methods: {
		checked(index) {
			this.isActive = index
		},
		getOrders(){
			uni.request({
				url:'http://localhost:80/api/orderlist.json',
				success: (res) => {
					this.items = res.data.orderlist
					console.log(this.items)
				}
			})
		}
	}
};
</script>

<style>
	.tab_nav{
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: #fff;
	}
	.tab_nav  .navTitle {
		height: 90rpx;
		line-height: 90rpx;
		width: 100%;
		text-align: center;
		font-size: 32rpx;
		font-family: Alibaba PuHuiTi;
		color: #333;
	}
	.active {
		position: relative;
		color: #1F75FE;
	}
	.active::after {
		content: "";
		position: absolute;
		width: 100rpx;
		height: 4rpx;
		background-color: #1F75FE;
		left: 0px;
		right: 0px;
		bottom: 0px;
		margin: auto;
	}

</style>
