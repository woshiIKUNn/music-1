// 主界面 搜索歌曲 轮播图 四个音乐榜单
<template>
	<view class="index">
		<musicHead title="网易云音乐" :icon="false"></musicHead>
		<view class="container">
			<scroll-view scroll-y="true" v-for="item in 10">
				<view class="index-search" @tap="handleToSearch">
					<text class="iconfont icon-search"></text>
					<input type="text" placeholder="搜索歌曲" />
				</view>
				<!-- 轮播图 -->
				<view class="banner">
					<swiper 
					:indicator-dots="true" 
					indicator-active-color="#ff372b"
					:autoplay="true" 
					:interval="3000" 
					:duration="500">
						<swiper-item>
							<view class="swiper-item">
								<img src="http://p1.music.126.net/7rrn_0ksZwxofTSNzS_y8w==/109951169157628102.jpg?imageView&quality=89" class="img" />
							</view>
						</swiper-item>
						<swiper-item>
							<view class="swiper-item">
								<img src="http://p1.music.126.net/uo5ueBMk10flI6iMxZtvFw==/109951169157512551.jpg?imageView&quality=89" class="img" />
							</view>
						</swiper-item>
						<swiper-item>
							<view class="swiper-item">
								<img src="http://p1.music.126.net/FfY_bJXw37snNfJQB2gh7w==/109951169157490996.jpg?imageView&quality=89" class="img" />
							</view>
						</swiper-item>
						<swiper-item>
							<view class="swiper-item">
								<img src="http://p1.music.126.net/1kQyLAhsw4PsGgewfpW9jA==/109951169157619242.jpg?imageView&quality=89" class="img" />
								</view>
						</swiper-item>
						<swiper-item>
							<view class="swiper-item">
								<img src="http://p1.music.126.net/DfRV7ZK2kE-AFB2NFx3X7A==/109951169157516969.jpg?imageView&quality=89" class="img" />
								</view>
						</swiper-item>
					</swiper>
				</view>
				<view v-if="isLoading">
					<m-for-skeleton 
					:avatarSize="200" 
					:row="3" 
					:loading="isLoading" 
					:isarc="'square'"
					v-for="(item,key) in 4" :key="key" 
					:titleStyle="{}" 
					:title="false">
					</m-for-skeleton>
				</view>
				
				<view class="index-list" v-else>
					<view class="index-list-item" v-for="(item,index) in toplist" :key="index"
						@tap="handleToList(item.id)">
						<view class="index-list-img">
							<image :src="item.coverImgUrl" mode="aspectFill"></image>
							<text>{{item.updateFrequency}}</text>
						</view>
						<view class="index-list-text">
							<view v-for="(item2,index2) in item.tracks" :key="index2">{{index2+1}}.{{item2.first}} -
								{{item2.second}}
							</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	import {
		topList
	} from '../../common/api.js'
	import mForSkeleton from "@/components/m-for-skeleton/m-for-skeleton";

	export default {
		data() {
			return {
				toplist: [],
				isLoading: true

			}
		},

		onLoad() {
			topList().then((res) => {
				if (res.length) {
					this.toplist = res;
					this.isLoading = false
				}
			})
		},
		methods: {
			handleToList(id) {
				uni.navigateTo({
					url: '/pages/list/list?id=' + id,
					success: res => {

					},
					fail: () => {},
					complete: () => {}
				});
			},
			handleToSearch() {
				uni.navigateTo({
					url: '/pages/search/search'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.index {}

	.index-search {
		display: flex;
		align-items: center;
		height: 70rpx;
		margin: 70rpx 30rpx 30rpx 30rpx;
		background: #f7f7f7;
		border-radius: 50rpx;
	}

	.index-search text {
		font-size: 26rpx;
		margin-right: 26rpx;
		margin-left: 28rpx;

	}

	.index-search input {
		font-size: 28rpx;
		flex: 1
	}

	.index-list {
		margin: 0 30rpx;
	}

	.index-list-item {
		display: flex;
		margin-bottom: 34rpx;
	}

	.index-list-img {
		width: 212rpx;
		height: 212rpx;
		position: relative;
		border-radius: 30rpx;
		overflow: hidden;
		margin-right: 10rpx;
	}

	.index-list-img image {
		width: 100%;
		height: 100%;
	}

	.index-list-img text {
		position: absolute;
		left: 12rpx;
		bottom: 16rpx;
		color: white;
		font-size: 20rpx;
	}

	.index-list-text {
		font-size: 24rpx;
		line-height: 66rpx;

		view {
			width: 50vw;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
		}
	}
	.img{
		width: 100%;
		height: 100%;
	}
</style>
