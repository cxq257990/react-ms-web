<template>
	<view class="vCommentBtnList">
		<view class="btn" v-for="item in btnList" :key="item.type" @click="onClick(item)">
			<template v-if="item.type=='forward'">
				<button class="share" :data-id="item.parentId" :data-obj="item.obj?item.obj:''" open-type="share">
					<view class="iconfont" :class="item.iconclass"></view>
					<text class="count">{{item.count}}</text>
				</button>
			</template>
			<template v-else>
				<view class="iconfont" :class="item.iconclass"></view>
				<text class="count">{{item.count}}</text>

			</template>

		</view>
	</view>
</template>

<script>
	export default {
		props: {
			btnList: {
				type: Array,
				default: () => {
					return []
				}
			}

		},
		data() {
			return {}
		},
		methods: {
			onClick(item) {
				if (item.type != 'forward') {
					this.$emit('emitForwardLike', item)
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.vCommentBtnList {
		border-top: 1rpx solid $uni-border-color;
		height: 84rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;



		.btn {
			margin-right: 18rpx;
			display: flex;
			align-items: center;

			.share {
				line-height: 1;
				background-color: #fff;
				border: none;

				display: flex;
				align-items: center;
			}

			.share::after {
				border: none;
			}

			.iconfont {
				// color: #444444;
				font-size: 38rpx;

			}

			.active {
				color: $uni-color-active;
			}

			.count {
				margin-left: 15rpx;
				font-size: 24rpx;
				font-family: Microsoft YaHei;
				font-weight: 400;
				color: $uni-text-color-greyA;
			}
		}
	}
</style>
