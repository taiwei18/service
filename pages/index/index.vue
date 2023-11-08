<template>
	<view>
		<view class="header">
			<image @click="goService" style="width: 100vw;" src="@/image/header.png" mode=""></image>
			<view class="flex-center" style="width: 90vw;margin: auto;">
				<u-search v-model="search" shape="square" height="50" bg-color="#f0f0f7" placeholder="请输入关键词搜索"
					:show-action="false"></u-search>
			</view>
		</view>
		<view class="main ">
			<view class="main-card " v-for="(item,index) in 6" @click="goDetails(index)">
				<text class="main-card_title">三抗生素软膏对鼻腔金黄色葡萄球菌定植是否有清除作用?</text>
				<view class="main-card_conten textellipsis-two">
					我们的研究中医务人员金黄色葡萄球菌的定植率低于已发表的数据但随着莫匹罗星耐药金黄色葡萄球菌的日益增多，鼻腔应用三抗抗
				</view>
				<view class="main-card_tag">
					<u-tag text="三抗生素软膏"  type="primary" />
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				search: null
			};
		},
		methods: {
			goService() {
				uni.navigateTo({
					url: '/pages/service/service'
				});
			},
			goDetails(){
				uni.navigateTo({
					url: '/pages/details/details'
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.header {
		background-color: white;
		padding-bottom: 30rpx;
	}

	.main {
		display: flex;
		flex-direction: column;
	}

	.main-card {
		width: 90vw;
		margin: auto;
		padding: 30rpx;
		margin-top: 30rpx;
		border-radius: 15rpx;
		background-color: white;
	}

	.main-card_title {
		color: black;
		font-size: 35rpx;
		font-weight: bolder;
	}

	.main-card_conten {
		margin-top: 10rpx;
	}

	.main-card_tag {
		display: flex;
		margin-top: 10rpx;
	}
</style>