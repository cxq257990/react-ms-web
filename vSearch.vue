<template>
	<view class="vSearch">
		<view class="iconfont icon-sousuo icon-search"></view>
		<input type="text" class="uni-input" confirm-type="search" placeholder-class="input-placeholder"
			:value="inputValue" placeholder="请输入关键词进行搜索" @input="onInput" @confirm="onConfirm" @focus="onFocus"
			@blur="onBlur" />
		<view v-if="showDelete" class="iconfont icon-s-shanchutupian icon-cancel" @click="onClear"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showDelete: false,
				inputValue: '',
			}
		},
		methods: {
			clear(){
				this.inputValue = ""
			},
			onInput(event) {
				this.inputValue = event.target.value
			},
			onFocus() {
				this.showDelete = true;
			},
			onBlur() {
				this.showDelete = false;
			},
			onConfirm() {
				this.$emit('emitSearch', {
					type: "ok",
					val: this.inputValue
				})
			},
			onClear() {
				this.inputValue = ""
				this.$emit('emitSearch', {
					type: "clear",
					val: this.inputValue
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.vSearch {
		padding: 0 30rpx;
		width: 100%;
		height: 88rpx;
		background: #F3F3F3;
		border-radius: 45rpx;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;

		.uni-input {
			flex: 1;
			height: 30rpx;
			font-size: 30rpx;
			font-family: Microsoft YaHei;
			font-weight: 400;
			color: $uni-text-color;
		}

		.input-placeholder {
			height: 30rpx;
			font-size: 30rpx;
			font-family: Microsoft YaHei;
			font-weight: 400;
			color: $uni-text-color-placeholder;
			text-align: left;
		}

		.iconfont {
			color: $uni-text-color;
			font-size: 36rpx;
		}

		.icon-search {
			margin-right: 15rpx;
			border: 3rpx solid #EAEAEA;
			border-radius: 50%;
		}

		.icon-cancel {
			margin-left: 15rpx;
		}
	}
</style>
