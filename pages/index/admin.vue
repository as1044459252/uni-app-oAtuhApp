<template>
	<view>
		<view class="container">尊敬的管理员您好。你的昵称为{{qqNumber}}</view>
	</view>
</template>

<script>
	export default {
		onLoad:function(option){
			if(option.token){
				//处理本地开发移动端和PC端localhost的问题
				let url = 'http://localhost:8081/checkLogin?token='+option.token;
				//#ifdef APP-PLUS
				url = url.replace('localhost','10.0.2.2');
				//#endif
				//#ifdef MP-WEIXIN
				url = url.replace('localhost','10.0.2.2');
				//#endif
				uni.request({
					url:url,
					success: (res) => {
						this.qqNumber = res.data.qqNumber;
						let role = res.data.role;
						if(role!="admin"){
							uni.navigateTo({
								url:"login",
							})
						}
							
					}
														
				})
			}
			
		},
		data() {
			return {
				qqNumber: '',
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
