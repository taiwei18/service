<template>
	<view>
		<view class="header">
			<view class="header-title">
				三抗生素软膏对鼻腔金黄色葡萄球菌定植是否有清除作用?
			</view>
			<view class="header-tag">
				<u-tag text="三抗生素软膏" bg-color="#007BC7" type="primary" />
			</view>
		</view>
		<view class="main">
			<view class="main-card" v-html="html">
			</view>
		</view>
<!-- 		<view class="video">
			<video id="myVideo" src="https://qiniu-web-assets.dcloud.net.cn/unidoc/zh/2minute-demo.mp4"></video>
		</view> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				html: '<p>选取了一家研究型医院中216名医务人员，通过鼻拭子进行定植的金黄色葡萄球菌的采集培养。其中45人(20.8%)金黄色葡萄球菌培养阳性，且3人(1.4%)表现为MRSA。这45名金黄色葡萄球菌培养阳性的医务人员中，30人每天两次鼻腔应用三抗生素软膏，持续5天。治疗1周后，重新采集30人的臭拭子培养，显示16人(53.3%)金黄色葡萄球菌的清除作用。</p><p><strong>结论:</strong> 我们的研究中医务人员金黄色葡萄球菌的定植率低于已发表的数据。1但随着莫匹罗星耐药金黄色葡萄球菌的日益增多，鼻腔应用三抗生素软膏可能是清除鼻腔金黄色葡萄球菌的有效可行的选择。</p>'
			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	page {
		padding: 20rpx;
		background-color: white;
	}

	.header-tag {
		display: flex;
		margin-top: 20rpx;
	}

	.main-card {
		padding: 35rpx;
		margin-top: 30rpx;
		border-radius: 20rpx;
		font-size: 30rpx;
		line-height: 50rpx;
		background-color: #e8f3fa;
		color: $uni-primary;
	}
</style>