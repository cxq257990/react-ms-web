<template>
	<view class="vNoResult">
		<image class="img" src="../../static/img/noResult.jpg" mode="aspectFit"></image>
		<view class="txt">{{txt}}</view>
	</view>
</template>

<script>
	export default {
		props: {
			txt: {
				type: String,
				default: ""
			}
		},
		data() {
			return {}
		}
	}
</script>

<style lang="scss" scoped>
	.vNoResult {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		z-index: 1;
		text-align: center;

		.img {
			width: 155rpx;
			height: 140rpx;
			margin-bottom: 10rpx;
		}

		.txt {
			color: $uni-text-color-greyA;
			font-size: 26rpx;
			text-align: center;
		}
	}
</style>
