<template>
	<view>
		<web-view :src="url"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: null
			}
		},
		onLoad(e) {
			console.log(e);
			this.url = e.url
		},
		methods: {

		}
	}
</script>

<style>

</style>