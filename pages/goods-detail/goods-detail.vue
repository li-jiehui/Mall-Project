<template>
	<!-- //    商品详细页    //  -->
	<view class="goods_detail">
		<swiper indicator-dots>
			<swiper-item v-for="(item,index) in swipers" :key="index">
				<image :src="item.src"></image>
			</swiper-item>
		</swiper>
		<view class="box1">
			<view class="price">
				<text>￥{{info.sell_price}}</text>
				<text>￥{{info.market_price}}</text>
			</view>
			<view class="goods_name">{{info.title}}</view>
		</view>
		<view class="line"></view>
		<view class="box2">
			<view>货号：{{info.goods_no}}</view>
			<view>库存：{{info.stock_quantity}}</view>
		</view>
		<view class="line"></view>
		<view class="box3">
			<view class="tit">详情介绍</view>
			<view class="content">
				<rich-text :nodes="content"></rich-text>
			</view>
		</view>
		<!-- 加入购物车导航栏 -->
		<view class="goods_nav">
	<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		data() {
			return {
				id: 0,
				swipers: [],
				info: {},
				content: '',
				options: [{
					icon: 'headphones',
					text: '客服'
				}, {
					icon: 'shop',
					text: '店铺',
					info: '',
					infoBackgroundColor: '#007aff',
					infoColor: "red"
				}, {
					icon: 'cart',
					text: '购物车',
					info: 0
				}],
				buttonGroup: [{
						text: '加入购物车',
						backgroundColor: '#ff0000',
						color: '#fff'
					},
					{
						text: '立即购买',
						backgroundColor: '#ffa200',
						color: '#fff'
					}
				]
			}
		},
		methods: {
			async getSwipers() {
				const res = await this.$myRuquest({
					url: '/api/getthumimages/' + this.id
				})
				this.swipers = res.data.message
			},
			async getDetailInfo() {
				const res = await this.$myRuquest({
					url: '/api/goods/getinfo/' + this.id
				})
				this.info = res.data.message[0]
			},
			async getDetailContent() {
				const res = await this.$myRuquest({
					url: '/api/goods/getdesc/' + this.id
				})
				this.content = res.data.message[0].content
			},
			onClick (e) {
				if(e.index == 2) {
					uni.switchTab({
						url: '../cart/cart'
					})
				} else {
					uni.showToast({
						title: `点击${e.content.text}`,
						icon: 'none'
					})
				}
			    },
			    buttonClick (e) {
					// console.log(e)
					// console.log(e.index)
					if(e.index == 0) {
						this.options[2].info++
					} else {
						uni.showToast({
							title: '已无存货',
							icon: 'none'
						})
					}
			    }
		},
		components: {
			uniGoodsNav
		},
		onLoad(options) {
			this.id = options.id
			this.getSwipers()
			this.getDetailInfo()
			this.getDetailContent()
		}
	}
</script>

<style lang="scss">
	.goods_detail {
		swiper {
			height: 700rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.box1 {
			padding: 10px;

			.price {
				font-size: 35rpx;
				color: $shop-color;
				line-height: 80rpx;

				text:nth-child(2) {
					color: #ccc;
					font-size: 28rpx;
					text-decoration: line-through;
					margin-left: 20rpx;
				}
			}

			.goods_name {
				font-size: 32rpx;
				line-height: 60rpx;
			}
		}

		.box2 {
			padding: 0 10px;
			font-size: 32rpx;
			line-height: 70rpx;
		}

		.box3 {
			padding-bottom: 100rpx;
			.tit {
				font-size: 32rpx;
				padding-left: 10rpx;
				border-bottom: 1px solid #eee;
				line-height: 70rpx;
			}

			.content {
				padding: 10rpx;
				font-size: 30rpx;
				line-height: 50rpx;
			}
		}
	}

	.goods_nav {
		position: fixed;
		bottom: 0;
		width: 100%;
	}
	
	.line {
		height: 10rpx;
		width: 750rpx;
		background: #eee;
	}
</style>
