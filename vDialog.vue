<template>
	<view class="vDialog">
		<view class="main">
			<view class="message">{{msg}}</view>
			<view class="btn-list">
				<view class="btn cancel-btn" @click="onCancel">取消</view>
				<view class="btn ok-btn" @click="onOk">确定</view>
			</view>
		</view>
		<view class="bg"></view>
	</view>
</template>

<script>
	export default {
		props: {
			msg: {
				type: String,
				default: ""
			}
		},

		data() {
			return {}
		},
		methods: {
			onCancel() {
				this.$emit('emitDialog', 'cancel')
			},
			onOk() {
				this.$emit('emitDialog', 'ok')
			}
		}
	}
</script>

<style lang="scss" scoped>
	.vDialog {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: 999999;

		.main {
			position: absolute;
			top: 50%;
			left: 50%;
			z-index: 9;
			transform: translate(-50%, -50%);
			width: 540rpx;
			background-color: #fff;
			border-radius: 26rpx;

			.message {
				color: $uni-text-color;
				font-size: 34rpx;
				padding: 20rpx 50rpx;
				min-height: 224rpx;
				display: flex;
				align-items: center;
				justify-content: center;
			}

			.btn-list {
				border-top: 1rpx solid $uni-border-color;
				display: flex;
				align-items: center;

				.btn {
					flex: 1;
					text-align: center;
					height: 88rpx;
					line-height: 88rpx;
					text-align: center;
				}

				.cancel-btn {
					color: $uni-text-color;
					border-right: 1rpx solid $uni-border-color;
				}

				.ok-btn {
					color: $uni-color-active;
				}
			}
		}

		.bg {
			width: 100%;
			height: 100%;
			background-color: #000000;
			opacity: 0.2;
		}
	}
</style>
