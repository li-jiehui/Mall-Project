<template>
	<!-- //   联系我们   // -->
	<view class="contact">
		<image src="../../static/xxlogo.png" class="img"></image>
		<view class="info">
			<view @click="phoneNumber">联系电话：131-3504-1050 (点击拨打)</view>
			<view>学校地址：广州市白云工商技师学院(广州市白云区江高镇田南路13号)</view>
		</view>
		<map class="map" :longitude="longitude" :latitude="latitude" :markers="markers" :scale="scale"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				longitude: 113.213848,
				latitude: 23.275232,
				scale: 15,
				markers: [
					{
						longitude: 113.213848,
						latitude: 23.270232,
						iconPath: '../../static/xxlogo.png',
						width: 30,
						height: 25
					}
				]
			}
		},
		methods: {
			phoneNumber() {
				uni.makePhoneCall({
					phoneNumber: '131-3504-1050'
				})
			}
		}
	}
</script>

<style lang="scss">
.contact{
	.img{
		width: 750rpx;
		height: 350rpx;
	}
	.info {
		padding: 10rpx 20rpx;
		font-size: 30rpx;
		view{
			border-bottom: 1px solid #eee;
			line-height: 80rpx;
		}
	}
	.map{
		width: 750rpx;
		height: 750rpx;
	}
}
</style>
