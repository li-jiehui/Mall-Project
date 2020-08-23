<template>
	<!-- ///     资讯页           -->
	<view class="news">
		<news-item @itemClick="geDetail" :list="newsList"></news-item>
	</view>
</template>

<script>
	import newsItem from '../../components/news-item/news-item.vue'
	export default {
		data() {
			return {
				newsList: []
			}
		},
		methods: {
			async getnews() {
				const res = await this.$myRuquest({
					url: '/api/getnewslist'
				})
				this.newsList = res.data.message
				// console.log(res)
			},
			geDetail (id) {
				uni.navigateTo({
					url: '/pages/news-detail/news-detail?id='+id
				})
			}
		},
		components:{
			'news-item':newsItem
		},
		onLoad() {
			this.getnews()
		}
	}
</script>

<style lang="scss">
.news{
	padding-bottom: 50rpx;
}
</style>
