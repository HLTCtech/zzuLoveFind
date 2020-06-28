<template>
	<view class="index" style="margin-top: 300rpx;">
		<block v-for="(list, index) in lists" :key="index">
			<view class="row">
				<view class="card card-list2" v-for="(item,key) in list" @click="goDetail(item)" :key="key">
					<image class="card-img card-list2-img" :src="item.img_src"></image>
					<!-- <text class="card-num-view card-list2-num-view">{{item.img_num}}P</text> -->
					<view class="card-bottm row">
						<view class="car-title-view row">
							<image src="/static/images/girls.png" class="iconMini"></image>
							<text class="card-title card-list2-title" style="margin-left: 10rpx;">{{item.title}}</text>
						</view>
						<!-- <view @click.stop="share(item)" class="card-share-view"></view> -->
					</view>
				</view>
			</view>
		</block>
		<text class="loadMore">加载中...</text>
	</view>
</template>

<script>
	// import '@/common/common.css'
	export default {
		data() {
			return {
				refreshing: false,
				lists: [],
				fetchPageNum: 1
			}
		},
		onLoad() {
			this.getData();
		},
		onPullDownRefresh() {
			console.log("下拉刷新");
			this.refreshing = true;
			this.getData();
		},
		onReachBottom() {
			this.getData();
		},
		methods: {
			getData() {
				uni.request({
					url: 'https://unidemo.dcloud.net.cn' + '/api/picture/posts.php?page=' + (this.refreshing ? 1 : this.fetchPageNum) + '&per_page=10',
					success: (ret) => {
						if (ret.statusCode !== 200) {
							console.log("请求失败:", ret)
						} else {
							if (this.refreshing && ret.data[0].id === this.lists[0][0].id) {
								uni.showToast({
									title: "已经最新",
									icon: "none",
								})
								this.refreshing = false;
								uni.stopPullDownRefresh()
								return;
							}
							let list = [],
								lists = [],
								data = ret.data;
							for (let i = 0, length = data.length; i < length; i++) {
								let index = Math.floor(i / 2);
								list.push(data[i]);
								if (i % 2 == 1) {
									lists.push(list);
									list = [];
								}
							}
							console.log("得到lists", lists);
							if (this.refreshing) {
								this.refreshing = false;
								uni.stopPullDownRefresh()
								this.lists = lists;
								this.fetchPageNum = 2;
							} else {
								this.lists = this.lists.concat(lists);
								this.fetchPageNum += 1;
							}
						}
					}
				});
			},
			goDetail(e) {
				uni.navigateTo({
					url:"../detail/detail?data=" + encodeURIComponent(JSON.stringify(e))
				})
			},
		}
	}
</script>

<style>
	view {
	    display: flex;
	}
	
	page {
	    display: flex;
	    min-height: 100%;
	    background-color: #EFEFEF;
	}
	
	template {
	    display: flex;
	    flex: 1;
	}
	
	.iconMini {
		height: 30rpx;
		width: 30rpx;
	}
	
</style>
