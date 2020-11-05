<template>
	<view class="container">
		你已经登录成功。你的昵称为{{qqNumber}}
		<view class="container">
			<button @click="toUser">去普通用户界面</button>		
			<button @click="toAdmin">管理员界面</button>			
		</view>
	</view>
	
</template>

<script>
	export default {
		onLoad:function(option){
			if(option.login=='success'){
				//处理本地开发移动端和PC端localhost的问题
				let url = 'http://localhost:8081/getLoginInfo';
				//#ifdef APP-PLUS
				url = url.replace('localhost','10.0.2.2');
				//#endif
				//#ifdef MP-WEIXIN
				url = url.replace('localhost','10.0.2.2');
				//#endif
				
				uni.request({
					url: url,
					data: {},
					success: (res) => {
						console.log(res.data);
						this.qqNumber = res.data.qqNumber;
						this.token = res.data.token;
						
						uni.redirectTo({
							url:"index?token="+this.token,
						})
					}
				})
			}
			//链接中带有token
			if(option.token){
				let url = 'http://localhost:8081/checkLogin?token='+option.token;
				//#ifdef APP-PLUS
				url = url.replace('localhost','10.0.2.2');
				//#endif
				//#ifdef MP-WEIXIN
				url = url.replace('localhost','10.0.2.2');
				//#endif
				
				uni.request({
					url: url,
					data: {},
					success: (res) => {
						console.log(res.data);
						this.qqNumber = res.data.qqNumber;
						this.token = res.data.token;
					}
				})
				
			}
		},
		data() {
			return {
				qqNumber : '',
				token: '',
			}
		},
		methods: {
			isPC() {
			        var userAgentInfo = navigator.userAgent;
			        var Agents = ["Android", "iPhone",
			            "SymbianOS", "Windows Phone",
			            "iPad", "iPod"];
			        var flag = true;
			        for (var v = 0; v < Agents.length; v++) {
			            if (userAgentInfo.indexOf(Agents[v]) > 0) {
			                flag = false;
			                break;
			            }
			        }
			        return flag;
			    },
				toUser(){
					uni.navigateTo({
						url:"user?token="+this.token,
					})
				},
				toAdmin(){
					uni.navigateTo({
						url:"admin?token="+this.token,
					})
				},
				
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
