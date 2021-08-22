<template>
	<view class="vActionSheet">
		<view class="main">
			<view class="item" :class="item.type" v-for="(item,index) in arr" :key="item.type" @click="onClick(item)">
				{{item.txt}}
			</view>
		</view>
		<view class="bg"></view>
	</view>
</template>

<script>
	export default {
		props: {
			arr: {
				type: Array,
				default: () => {
					return [{
						type: "",
						txt: ""
					}]
				}
			}
		},

		data() {
			return {

			}
		},
		methods: {
			onClick(item) {
				this.$emit("emitActionSheet", item)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.vActionSheet {
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100vw;
		height: 100vh;	
		z-index: 99999;

		.main {
			position: absolute;
			z-index: 2;
			left: 0;
			bottom: 0;		
			width: 100%;
			font-size: 34rpx;
			background-color: #fff;
			box-sizing: border-box;			
			padding: 0 44rpx;
			color: $uni-text-color;

			.item {
				height: 100rpx;
				line-height: 100rpx;
				text-align: center;
				
				border-bottom: 1rpx solid  $uni-border-color;
				
			}

			.item:last-of-type {
				border-bottom: none;
			}
			.cancel {
				color: $uni-color-active;
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
