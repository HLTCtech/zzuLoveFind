<template>
	<view class="container">
		<view class="d-flex w-100 flex-column just-content-center align-items-center" style="margin-top: 150rpx;">
			<image src="/static/images/searching.gif" class="drinks-img"></image>
			<view class="tips d-flex flex-column align-items-center font-size-base text-color-assist">
				<view>刚刚擦肩而过的Ta让你心跳漏了一拍？</view>
				<view>想去要微信又不知道怎么开口？</view>
				<view>如果Ta恰好也注意到你了呢？</view>
			</view>
			<button type="primary" class="drink-btn" size="default" @tap="menu">去试试</button>
			<view class="font-size-sm text-color-primary" @tap="orders">查看功能介绍</view>
			
			
		</view>
	</view>
</template>

<script>
	import listCell from '@/components/list-cell/list-cell'
	import {mapState} from 'vuex'
	
	export default {
		components: {
			listCell
		},
		data() {
			return {
				
			}
		},
		computed: {
			...mapState(['order'])
		},
		methods: {
			orders() {
				if(!this.$store.getters.isLogin) {
					uni.navigateTo({url: '/pages/login/login'})
					return
				}
				uni.navigateTo({
					url: '/pages/orders/orders'
				})
			},
			menu() {
				uni.switchTab({
					url: '/pages/menu/menu'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	/* #ifdef H5 */
	page {
		min-height: 100%;
		background-color: $bg-color;
	}
	/* #endif */
	.order-box {
		padding: 20rpx;
		/* #ifdef H5 */
		margin-bottom: 100rpx;
		/* #endif */
	}
	
	.drinks-img {
		width: 600rpx;
		height: 600rpx;
	}
	
	.tips {
		margin: 60rpx 0 80rpx;
		line-height: 48rpx;
	}
	
	.drink-btn {
		width: 320rpx;
		border-radius: 50rem !important;
		margin-bottom: 40rpx;
		font-size: $font-size-base;
		line-height: 3.0;
	}
	
	@mixin arch {
		content: "";
		position: absolute;
		background-color: $bg-color;
		width: 30rpx;
		height: 30rpx;
		bottom: -15rpx;
		z-index: 10;
		border-radius: 100%;
	}
	
	.section {
		position: relative;
		
		&::before {
			@include arch;
			left: -15rpx;
		}
		
		&::after {
			@include arch;
			right: -15rpx;
		}
	}
	
	.pay-cell {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: $font-size-base;
		color: $text-color-base;
		margin-bottom: 40rpx;

		&:nth-last-child(1) {
			margin-bottom: 0;
		}
	}
	
	.sort-num {
		font-size: 64rpx;
		font-weight: bold;
		color: $text-color-base;
		line-height: 2;
	}
	
	.steps__img-column {
		display: flex;
		margin: 30rpx 0;
		
		.steps__img-column-item {
			flex: 1;
			display: flex;
			align-items: center;
			justify-content: center;
			
			image {
				width: 80rpx;
				height: 80rpx;
			}
		}
	}
	
	.steps__text-column {
		display: flex;
		margin-bottom: 40rpx;
		
		.steps__text-column-item {
			flex: 1;
			display: inline-flex;
			display: flex;
			align-items: center;
			justify-content: center;
			font-size: $font-size-base;
			color: $text-color-assist;
			
			&.active {
				color: $text-color-base;
				font-weight: bold;
				
				.steps__column-item-line {
					background-color: $text-color-base;
				}
			}
			
			.steps__column-item-line{
				flex: 1;
				height: 2rpx;
				background-color: #919293;
				transform: scaleY(0.5);
			}
			
			.steps__text-column-item-text {
				margin: 0 8px;
			}
		}
	}
</style>
