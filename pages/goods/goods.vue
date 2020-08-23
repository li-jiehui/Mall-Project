<template>
	<!-- //   商城    //-->
	<view class="goods_list">
		<goods-list @goodsItemClick="goGoodsDetail" :goods="goods"></goods-list>
		<view class="isOver" v-if="flag">-----我是有底线的-----</view>
	</view>
	
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				pageindex:1,
				goods:[],
				flag:false
			}
		},
		components:{
				"goods-list":goodsList
		},
		methods: {
			//获取商品列表的数据
			async getgoodslist(callBack) {
				const res = await this.$myRuquest({
					url:'/api/getgoods?pageindex='+this.pageindex
				})
				// console.log(res)
				this.goods = [...this.goods,...res.data.message]
				callBack && callBack()
			},
			//跳转到商品详情页
			goGoodsDetail (id) {
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?id='+id
				})
			}
		},
		onLoad() {
			this.getgoodslist();
		},
		onReachBottom() {
			if(this.goods.length < this.pageindex*10) return this.flag = true
			// console.log("上拉触底了")
			this.pageindex++
			this.getgoodslist()
		},
		onPullDownRefresh() {
			// console.log("下拉刷新了")
			this.pageindex = 1
			this.goods = []
			this.flag = false
			setTimeout(()=>{
				this.getgoodslist(()=>{
					uni.stopPullDownRefresh()
				})
			},1000)
		}
	}
</script>

<style lang="scss">
.goods_list{
	background: #eee;
}
.isOver{
	width: 100%;
	height: 50px;
	line-height: 50px;
	text-align: center;
	font-size: 28rpx;
}
</style>
