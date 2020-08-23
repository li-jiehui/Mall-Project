<template>
	<!-- 资讯详情页 -->
	<view class="news_detail">
		<text class="title">{{detail.title}}</text>
		<view class="info">
			<text>发表时间：{{detail.add_time | formatDate}}</text>
			<text>浏览：{{detail.click}}</text>
		</view>
		<view class="content">
			<rich-text :nodes="detail.content"></rich-text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				detail: {}
			}
		},
		methods: {
			async getNewsDetail() {
				const res = await this.$myRuquest({
					url: '/api/getnew/'+this.id
				})
				this.detail = res.data.message[0]
				console.log(this.id)
			}
		},
		onLoad(options) {
			this.id = options.id
			this.getNewsDetail()
		}
	}
</script>

<style lang="scss">
.news_detail{
	font-size: 30rpx;
	padding: 0 20rpx 100rpx 20rpx;
	.title{
		text-align: center;
		width: 710rpx;
		display: block;
		margin: 20rpx 0;
		font-size: 40rpx;
		font-weight: bold;
	}
	.info{
		display: flex;
		justify-content: space-between;
		color: #999999;
	}
	.content{
		margin-top: 30rpx;
	}
}
</style>
