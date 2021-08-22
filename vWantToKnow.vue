<template>
	<view class="wantToKnow">
		<view class="gap"></view>
		<view class="wantToKnow-header padding-aside">
			猜您可能还想认识
		</view>
		<view class="wantToknow-line"></view>
		<view class="wantToKnow-list padding-aside" v-for="(item,index) in wantToKnowList" :key="index">
			<view class="info-box">
				<view class="icon-box" @tap="toInfoChild(item.states,item.open_id,item.can_phone,item.outer_id)">
					<view class="avatar">
						<view v-if="item.new_flag == 1" class="unread-icon">new</view>
						<image v-if="item.avatar" :src="item.avatar" mode="widthFix" class="left"></image>
						<image v-else-if="item.sex=='男'" src="../../static/img/man.png" mode="widthFix" class="left"></image>
						<image v-else-if="item.sex=='女'" src="../../static/img/woman.png" mode="widthFix" class="left"></image>
						<image v-else src="../../static/img/anonymity.png" mode="widthFix" class="left"></image>
					</view>
					<view class="info">
						<view class="nameAndPosition">
							<view v-if="item.user_name" class="name">{{item.user_name}}</view>
							<view v-if="item.position_name" class="position">{{item.position_name}}</view>
						</view>
						<view v-if="item.company_name" class="company">{{item.company_name}}</view>
					</view>
				</view>
				<view class="right">
					<view v-if="item.states == 0" class="iconfont icon-jiahaoyouzhong icon-color"></view>
					<view v-else-if="item.states == 1" class="iconfont icon-tianjiahaoyou1 icon-color"></view>
					<view v-else class="iconfont icon-jiahaoyou1 icon-color" @tap="addThisfriendChild(item.open_id,item.can_phone)"></view>
				</view>
			</view>
			<view class="together-friend">
				<view v-if="item.div == 2" class="iconfont icon-gongtonghaoyou2 icon-color self-color"></view>
				<view v-if="item.div == 3" class="iconfont icon-gongtonghaoyou3 icon-color self-color"></view>
				<view v-if="item.div == 2" class="together-num">二度人脉-共同好友{{item.div_count}}人</view>
				<view v-if="item.div == 3" class="together-num">三度人脉</view>
			</view>
			<view class="border-bottom"></view>
		</view>
		<view class="noResultContainer" v-if="wantToKnowList.length == 0">
			<vNoResult txt='暂无数据'></vNoResult>
		</view>
		
	</view>
</template>

<script>
	import vNoResult from "components/common/vNoResult"
	import {
		mapState
	} from "vuex";
	export default {
		props: {
			outId: {
				type: String,
				default: () => {
					return ''
				}
			}
		},
		components: {
			vNoResult
		},
		data() {
			return {
				wantToKnowList: [],	//猜你认识列表
			}
		},
		computed: {
			...mapState(['loginInfoObj'])
		},
		mounted() {
			console.log(this.outId,'看下传递进来的参数')
			this.getWantToKnowList()
		},
		methods: {
			// 获取可能认识的人列表
			async getWantToKnowList() {
				let params = {
					openId: this.loginInfoObj.open_id,
					memberId: this.loginInfoObj.id,
					outId: this.outId
				}
				let res = await this.$http.getHasLoad('/zxxt/recommend/personalPenetration', params)
				let wantToKnowList = res.data.wx_parameter_vos
				this.wantToKnowList = wantToKnowList
			},
			toInfoChild(states,open_id,can_phone,outer_id) {
				// 进入详情 states =  0: 申请中，1：同意，2：忽略
				if (states == 1) { //好友 > 好友详情
					uni.navigateTo({
						url: '/pages/multiEntry/information?openId=' + open_id
					});
				} else { //非好友 > 猜你认识
					uni.navigateTo({
						url: '/pages/multiEntry/guessYoudo?phone=' + can_phone +
							'&hisopenid=' + open_id + '&flag=0' + '&outerid=' + outer_id
					});
				}
			},
			async addThisfriendChild(open_id,can_phone) {
				var params = {
					open_id: this.loginInfoObj.open_id,
					re_open_id: open_id,
					can_phone: can_phone
				}
				
				if ((!params.re_open_id) && (!params.can_phone)) {
					return uni.showToast({
								title: '添加好友缺少手机号码或者id',
								icon: 'none',
								duration: 2000
							})
				} 
				let res = await this.$http.postHasLoad('/zxxt/user/addFriend', params);
				// 判断是列表中的好友添加
				if (params.can_phone) {
					this.wantToKnowList.forEach(item =>{
						if(params.can_phone == item.can_phone) {
							item.states = 0
						}
					})
				}
				uni.showToast({
					title: '请求已发送',
					icon: 'none',
					duration: 2000
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	// 猜你认识样式
	.wantToKnow {
		width: 100%;
		padding-bottom: 188rpx;
		background-color: #f3f3f3;
		.gap {
			height: 32rpx;
			background-color: #f3f3f3;
		}
		.wantToKnow-header {
			padding: 34rpx 0;
			color: #209072;
			font-size: 34rpx;
			background-color: #fff;border-radius: 20rpx 20rpx 0 0;
		}
		.wantToknow-line {
			height: 1rpx;
			margin:  0 32rpx;
			background-color: #e0e0e0;
		}
		.padding-aside {
			padding-left: 32rpx;
			padding-right: 32rpx;
		}
		.wantToKnow-list  {
			padding: 36rpx 0 0 0;background-color: #fff;
			.icon-color {
				color: #209072;
				font-size: 50rpx;
			}
			.self-color {
				font-size: 40rpx;
			}
			.info-box {
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 0 32rpx;
				.icon-box {
					display: flex;
					justify-content: flex-start;
					align-items: center;
					.avatar {
						position: relative;
						.unread-icon {
							width: 51rpx;
							position: absolute;
							top: 2rpx;
							right: -12rpx;
							background-color: #F64135;
							line-height: 18rpx;
							height: 24rpx;
							box-sizing: border-box;
							border-radius: 12rpx;
							border: 2rpx solid #fff;
							display: flex;
							align-items: center;
							justify-content: center;
							font-size: 18rpx;
							color: #fff;
						}
						.left {
							width: 80rpx !important;
							height: 80rpx;
							border-radius: 50%;
						}
					}
					.info {
						margin-left: 20rpx;
						.nameAndPosition {
							display: flex;
							justify-content: flex-start;
							align-items: center;
							.name {
								width: 176rpx;
								font-size: 34rpx;
								font-weight: 700;
								color: #333;
								overflow: hidden;
								white-space: nowrap;
								text-overflow: ellipsis;
								// margin-right: 24rpx;
							}
							.position {
								max-width: 246rpx;
								// max-width: 308rpx;
								font-size: 24rpx;
								color: #999999;
								overflow: hidden;
								white-space: nowrap;
								text-overflow: ellipsis;
							}
						}
						.company {
							max-width: 430rpx;
							font-size: 28rpx;
							color: #999999;
							overflow: hidden;
							white-space: nowrap;
							text-overflow: ellipsis;
						}
					}
				}
			}
			.together-friend {
				padding: 0 130rpx;
				display: flex;
				justify-content: flex-start;
				align-items: center;
				margin-top: 14rpx;
				.together-num {
					font-size: 28rpx;
					color: #999999;
					margin-left: 12rpx;
				}
			}
			.border-bottom {
				height: 2rpx;
				margin: 0 32rpx;
				background-color: #e0e0e0;
				margin-top: 36rpx;
			}
		}
		.noResultContainer {
			position: relative;
			min-height: 300rpx;
			background-color: #fff;
		}
	}
</style>
