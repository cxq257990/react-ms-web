<template>
	<view class="status_bar" :style="{height:statusBarHeight+titleBarHeight+'px'}">
		<view class="status" :style="{height:statusBarHeight+'px'}"></view>
		<view class="title" :style="{height:titleBarHeight+'px'}">
			<image class="img" src="../../static/img/logo-top.png" mode=""></image>
			<view class="txt">{{txt}}</view>
		</view>
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
		data: () => {
			return {
				statusBarHeight: 0, //状态栏（时间，电量）
				titleBarHeight: 0 // 导航栏 （头部显示文字）

			}
		},

		created() {
			let res = uni.getSystemInfoSync();
			this.statusBarHeight = res.statusBarHeight;
			let menu = wx.getMenuButtonBoundingClientRect();
			//获取获取菜单按钮（右上角胶囊按钮）的布局位置信息。坐标信息以屏幕左上角为原点。（top表示上边框到手机顶部的距离 bottom是下边框到手机顶部的距离）
			console.log('menu:' + menu + '这个？' + res.statusBarHeight)
			this.titleBarHeight = (menu.top - res.statusBarHeight) * 2 + menu.height;
			if (res.model.indexOf('iPhone') > -1) {
				this.titleBarHeight += 4
			}
		},
	}
</script>
<style lang="scss" scoped>
	.status_bar {
		position: fixed;
		top: 0;
		z-index: 999999;
		width: 100%;
		background-color: #fff;

		.title {
			display: flex;
			align-items: center;
			justify-content: center;

			.img {
				width: 108rpx;
				height: 48rpx;
				position: absolute;
				left: 24rpx;
				bottom: 20rpx;

			}

			.txt {
				font-size: 34rpx;
				font-weight: bold;
			}
		}

	}
</style>
