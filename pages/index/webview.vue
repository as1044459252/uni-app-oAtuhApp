<template>
	<view class="container">
		<web-view :src="url" @message="handleMessage"></web-view>
	</view>
</template>

<script>
	export default {
		onLoad:function(option){
			this.url = decodeURIComponent(option.url).replace('localhost','10.0.2.2');			
		},
		data() {
			return {
				url : '',
			}
		},
		methods: {
			handleMessage(evt){
				let msg = evt.detail.data;
				uni.navigateTo({
					url:"index?login=success",
				})
			}
		}
	}
</script>
<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
</style>
