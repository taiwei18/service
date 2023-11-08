<template>
	<view>
		<view class="header">
			<view class="header-drug">
				<view>
					<text class="header-drug_name">复方多粘菌素B软膏</text>
					<view class="header-drug_tag">
						<u-tag size="mini" text="处方药" type="primary" />
						<u-tag size="mini" text="外用药物" type="primary" />
					</view>
				</view>
				<view class="header-kf flex-center">
					<image src="@/image/kf.png" mode=""></image>
				</view>
			</view>
			<view class="header-explain">
				<text>共有1个相关药品说明书</text>
				<image style="width: 40rpx;height: 40rpx;" src="@/image/arrows.png" mode=""></image>
			</view>
		</view>
		<view class="main">
			<view class="tab-list flex-alignCenter">
				<view v-for="(item,index) in tabsList"
					:class="[index===tabsCurrent?'tab-list_item-active':'tab-list_item']"
					@click="handleTbs(index,item.name)">
					{{item.name}}
				</view>
			</view>
			<view class="main-content">
				<text class="main-content_title">
					{{tabsName}}
				</text>
				<view class="main-content-card" style="white-space:pre-wrap" v-html="content">
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				drugId: '',
				tabsCurrent: 0,
				tabsName: '适应症与用法用量',
				content: '<p>[适应症]用于预防皮肤割伤、擦伤、烧烫伤、手术伤口等皮肤创面的细菌感染和临时解除疼痛和不适。</p><p><br></p><p>[用法用量] 外用，局部涂于患处。一日2~4次，5天为一疗程。</p><p><br></p><p>[不良反应] 偶见过敏反应、搔痒、烧灼感、红肿等。</p><p><br></p><p>[禁忌]对本品任一组分过敏者禁用。</p><p><br></p><p>[注意事项]1.应避免在大面积烧伤面、肉芽组织或表皮脱落的巨大创面使用本品。2.当患者有肾功能减退或全身应用其它肾毒性或耳毒性药物时，应注意有产生毒性的可能。3.患者如有血屎、排屎次数减少、尿量减少或增多等肾毒性症状或耳鸣、听力减退等耳毒性症状时应慎用本品。4.本制剂不适于眼内使用。</p><p><br></p><p>[特殊人群用药]儿童注意事项:两岁以下儿童用药需遵医嘱。</p><p><br></p><p>妊娠与哺乳期注意事项:尚不明确。</p><p><br></p><p>老人注意事项:尚不明确。</p>',
				tabsList: [{
					name: '适应症与用法用量'
				}, {
					name: '安全用药'
				}, {
					name: '药理毒理'
				}]
			}
		},
		onLoad(e) {
			this.drugId = e.id
		},
		methods: {
			handleTbs(index, name) {
				this.tabsCurrent = index
				this.tabsName = name
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		padding: 30rpx;
		background-color: white;
	}

	.header {}

	.header-drug {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.header-drug_name {
		font-size: 48rpx;
		font-weight: bold;
	}

	.header-drug_tag {
		display: flex;
		align-items: center;
		margin-top: 30rpx;
		@include gap(10px);
	}

	.header-kf {
		width: 80rpx;
		height: 80rpx;
		border-radius: 20rpx;
		background-color: #e6eeff;

		image {
			width: 50rpx;
			height: 50rpx;
		}
	}

	.header-explain {
		width: 100%;
		height: 100rpx;
		border-radius: 20rpx;
		background: #f0f0f7;
		margin-top: 15px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0 30rpx;

		text {
			font-size: 30rpx;

			::before {
				content: '';
				display: inline-block;
				width: 15rpx;
				height: 15rpx;
				border-radius: 50%;
				margin-right: 10rpx;
				border: 5rpx solid #007BC7;
			}
		}
	}

	.tabs {
		margin-top: 30rpx
	}

	.tab-list {
		@include gap(60rpx);
	}

	.tab-list_item {}

	.tab-list_item-active {
		position: relative;

		&::before {
			content: '';
			width: 100%;
			height: 8rpx;
			border-radius: 10rpx;
			position: absolute;
			bottom: -60%;
			background-color: $uni-primary;
		}
	}

	.main {
		margin-top: 30rpx;
	}

	.main-content {
		margin-top: 80rpx;
	}

	.main-content_title {
		font-size: 40rpx;
		font-weight: bold;
		position: relative;
		z-index: 9999;

		&::before {
			content: '';
			width: 100%;
			height: 18rpx;
			position: absolute;
			bottom: 0;
			opacity: 0.8;
			z-index: -1;
			background-color: $uni-primary;
		}
	}

	.main-content-card {
		padding: 30rpx;
		margin-top: 30rpx;
		border-radius: 20rpx;
		font-weight: bold;
		background-color: #f9f9fc;
	}
</style>