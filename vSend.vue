<template>
	<view class="vSend">
		<view class="main">
			<view class="id-Pop-ups" v-if="showIDSelect">
				<view class="id-list">
					<view class="item" v-for="item in roleArr" :key="item.roleType" @click="onSelectID(item)">
						<!-- <view class="avatar iconfont icon-gongsi" v-if="item.roleType==3"></view> -->
						<image class="avatar" v-if="item.roleType==3" src="../../static/img/company.jpg" mode="">
						</image>
						<image class="avatar" v-else-if="item.roleType==2" src="../../static/img/anonymity.png" mode="">
						</image>
						<image class="avatar" v-else :src="userInfoObj.avatarUrl" mode=""></image>
						<text class="name">{{item.openName}}</text>
						<view class="iconfont icon-s-OK"></view>
					</view>
				</view>
				<view class="triangle"></view>
			</view>
			<view class="default-show">
				<view class="id-container" @click="onToggleIDSelect">
					<image class="avatar" v-if="selectIDObj.roleType==3" src="../../static/img/company.jpg" mode="">
					</image>
					<image class="avatar" v-else-if="selectIDObj.roleType==2" src="../../static/img/anonymity.png"
						mode="">
					</image>
					<image class="avatar" v-else :src="userInfoObj.avatarUrl" mode=""></image>

					<!-- <image class="avatar" src="../../static/img/anonymity.png" mode=""></image> -->
					<view class="arrow iconfont icon-s-xiangshang"></view>
				</view>
				<view class="input-container">
					<template v-if="!showTips || val.length>0">
						<input class="input" type="text" auto-focus="true" v-model="val" @input="onInput" />
					</template>
					<view v-else class="tips-con" @click="onEdit">
						<view class="iconfont icon-i-huifukuangdehuifu"></view>
						<view class="tips">我来说几句</view>
					</view>


				</view>
				<view class="emoji-btn iconfont icon-fabiaoqing" @click="onToggleEmoji"></view>
				<view class="send-btn" @click="onSend">发送</view>
			</view>
			<view class="emoji-list" v-if="showEmoji">
				<view class="item-row" v-for="(item,index) in emojiList" :key="index">
					<view class="opt" v-for="opt in item" :key="opt" @click="onSelectEmoji(opt)">
						{{opt}}
					</view>
				</view>
			</view>
		</view>
		<view class="bg" v-if="showBg"></view>
	</view>

</template>

<script>
	import emojiArr from "utils/emoji.js"
	import {
		mapState
	} from "vuex";
	export default {
		data() {
			return {
				showBg: false,

				showIDSelect: false,
				selectIDObj: {
					roleType: "2",
				},

				showTips: true,
				val: "",

				showEmoji: false,
				emojiList: [],


			}
		},
		computed: {
			...mapState(['roleArr', 'userInfoObj', 'loginInfoObj'])
		},

		created() {
			this.emojiList = [...emojiArr];
			this.selectIDObj = this.roleArr.find(item => {
				return item.roleType == "2"
			})
			//console.log("this.selectIDObj:", this.selectIDObj)
		},
		methods: {
			onToggleIDSelect() {
				let fn = async () => {
					this.showIDSelect = !this.showIDSelect;
					if (this.showIDSelect || this.showEmoji) {
						this.showBg = true;
					} else {
						this.showBg = false;
					}

				};
				this.$util.checkIslogin(fn.bind(null));
			},
			onSelectID(item) {
				this.showIDSelect = false;
				if (this.showIDSelect || this.showEmoji) {
					this.showBg = true;
				} else {
					this.showBg = false;
				}
				this.selectIDObj = item;
			},

			onEdit() {
				this.showTips = false;
			},
			onInput(e) {
				this.val = e.target.value;
				if (this.val.length == 0) {
					this.showTips = true;
				}

			},

			onToggleEmoji() {
				this.showEmoji = !this.showEmoji;
				if (this.showIDSelect || this.showEmoji) {
					this.showBg = true;
				} else {
					this.showBg = false;
				}

			},
			onSelectEmoji(val) {
				this.val += val;
			},
			onSend() {
				let fn = async () => {
					if (!!this.val.trim()) {
						this.$emit("emitSend", {
							val: this.val,
							...this.selectIDObj
						})
						this.val = "";
						this.showIDSelect = false;
						this.showEmoji = false;
						this.showBg = false;


					}

				};
				this.$util.checkIslogin(fn.bind(null));
			}
		}
	}
</script>

<style lang="scss" scoped>
	.vSend {
		width: 100%;

		.main {
			width: 100%;
			position: relative;
			z-index: 9;


			.id-Pop-ups {
				position: relative;
				top: -20rpx;
				left: 50%;
				transform: translateX(-50%);
				width: 94%;

				.triangle {
					margin-left: 70rpx;
					width: 0;
					height: 0;
					border-left: 12rpx solid transparent;
					border-right: 12rpx solid transparent;
					border-top: 15rpx solid #fff;
				}

				.id-list {
					padding: 10rpx 50rpx;
					box-sizing: border-box;

					background: #FFFFFF;
					box-shadow: 0px 4px 16px 0px rgba(18, 18, 18, 0.19);
					border-radius: 12px;


					.item {
						height: 100rpx;
						display: flex;
						align-items: center;
						border-bottom: 1rpx solid $uni-border-color;

						.avatar {
							font-size: 50rpx;
							text-align: center;
							width: 60rpx;
							height: 60rpx;
							border-radius: 100rpx;
							display: flex;
							align-items: center;
						}

						.name {
							flex: 1;
							margin: 0 24rpx;
							font-size: 30rpx;
							font-family: Microsoft YaHei;
							font-weight: 400;
							color: $uni-text-color;
						}

						.icon-s-OK {
							color: #fff;
						}
					}

					.item:last-of-type {
						border-bottom: none
					}

					.item.active {
						.icon-s-OK {
							color: $uni-color-active;
						}
					}

				}


			}


			.default-show {
				width: 100%;
				padding: 15rpx 20rpx;
				box-sizing: border-box;
				background-color: #fff;
				display: flex;
				align-items: center;

				.id-container {
					display: flex;
					align-items: center;

					.avatar {
						width: 60rpx;
						height: 60rpx;
						border-radius: 100rpx;
						font-size: 50rpx;
					}

					.arrow {
						color: $uni-text-color-greyA;
						font-size: 32rpx;
						margin: 0 10rpx;
					}
				}

				.input-container {
					flex: 1;
					height: 73rpx;
					background: #F7F7F7;
					border: 1rpx solid $uni-border-color;
					border-radius: 36rpx;
					


					.tips-con {
						height: 100%;
						padding: 0 25rpx;
						display: flex;
						align-items: center;

						.iconfont {
							font-size: 32rpx;
							color: #6B6B6B;
							margin-right: 15rpx;
						}

						.tips {
							font-size: 30rpx;
							font-family: Microsoft YaHei;
							font-weight: 400;
							color: $uni-text-color-greyA;
						}
					}

					.input {
						padding: 0 25rpx;
						height: 100%;
						flex: 1;
					}
				}

				.emoji-btn {
					font-size: 52rpx;
					color: #696969;
					margin: 0 20rpx;
				}

				.send-btn {
					width: 120rpx;
					height: 72rpx;
					border-radius: 100rpx;
					display: flex;
					align-items: center;
					justify-content: center;
					background: $uni-color-active;
					color: #fff;

				}
			}

			.emoji-list {
				width: 100%;
				background-color: #F7F7F7;

				.item-row {
					width: 100%;
					padding: 10rpx 0;

					display: flex;
					align-items: center;

					.opt {

						font-size: 48rpx;
						text-align: center;
						width: 14.28%;
					}
				}
			}
		}


		.bg {
			position: fixed;
			top: 0;
			left: 0;
			z-index: 1;
			width: 100vw;
			height: 100vh;
			background: #000;
			opacity: 0.3;
		}
	}
</style>
