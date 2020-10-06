<template>
	<view class="container">你已经登录成功。你的昵称为{{qqNumber}}</view>
</template>

<script>
	export default {
		onLoad:function(option){
			this.qqNumber = option.qqNumber;
		},
		data() {
			return {
				qqNumber : '',
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
