<template>
	<view class="container">
		<view class="container">
			<text class="intro">这是一个基于Oauth2.0，uni-app，shiro，springboot的项目</text>
			<form @submit="login" @reset="">
				<view class="uni-form-item uni-column">
					<view class="oneInput">
					<uni-icons type="person-filled" class="icons" size="28"></uni-icons>
					<input class="uni-input" name="username" placeholder="请输入您的用户名" />
					</view>
				</view>
				<view class="uni-form-item uni-column">
					<view class="oneInput">
					<uni-icons type="eye-slash-filled" class="icons" size="28"></uni-icons>
					<input class="uni-input" name="password" placeholder="请输入您的密码" />
					</view>
				</view>
				<button form-type="submit">登录</button>
			</form>
		</view>
		<navigator url="register">
		<button>去注册</button>
		</navigator>
		<button @click="toQQ">使用QQ登录</button>
		<button open-type="getUserInfo" lang="zh_CN" @getuserinfo="toWx">使用微信登录</button>
		<button @click="toWv">使用web-view登录</button>
		
	</view>
</template>

<script>
	export default {
		onLoad:function(option){
			if(option.login=='suc'){
				uni.request({
					url: 'http://10.0.2.2:8081/checkLogin',
					success: (res) => {
						//console.log(res.data);
						if(res.data!='error'){
							let qqNumber = res.data;
							uni.navigateTo({
								url: './index?qqNumber='+qqNumber,
							})
						}
					},
					fail: () => {
						uni.showToast({
							title:"登录失败，请重试",
						})
					}
				})
			}		
		},
		data() {
			return {
				href: 'https://uniapp.dcloud.io/component/README?id=uniui',
				
			}
		},
		methods: {
			login(){
				uni.request({
					url: 'http://localhost:8081/login',
					data: {},
					success: (res) => {
						console.log(res);
					}
				})
			},
			toRegister(){
				uni.navigateTo({
					url:'./register'
				});
			},
			
			toQQ(){
				uni.request({
					url: 'http://localhost:8081/toOauthQQLogin',
					success: (res) => {
						let href = res.data;
						window.open(href);
					}
				})
				let intervalID = setInterval(checkLogin,1000);
				function checkLogin(){
					uni.request({
						url: 'http://localhost:8081/checkLogin',
						success: (res) => {
							if(res.data!=='error'){
								let qqNumber = res.data;
								uni.navigateTo({
									url: './index?qqNumber='+qqNumber,
								})
								clearInterval(intervalID);
							}
						}
					})
				}
			},
			toWx(){
				uni.login({
					provider:"weixin",
					success:function(loginRes){
						console.log(loginRes);
						uni.getUserInfo({
							provider:"weixin",
							success: (res) => {
								console.log(res);
								uni.navigateTo({
									url: './index?qqNumber='+res.userInfo.nickName,
								})
							},
							fail: () => {
								uni.showToast({
									title:"授权失败",
								})
							}
						})
					},
					fail: () => {
						uni.showToast({
							title:"授权失败",
						})
					}
				});
			},
			toWv(){		
				uni.request({
					url: 'http://10.0.2.2:8081/toOauthQQLogin',
					method: 'GET',
					success: (res) => {
						let href = res.data;
					    
						uni.navigateTo({
							url: './webview?url='+encodeURIComponent(href),
						})
					},
					fail: (res) => {
						console.log('登录失败');
					}
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
	button {
		width: 200rpx;
		margin-top: 10rpx;
		background-color: #FFFFFF;
		border-radius: 0px;
		border-style: solid;
		border-color: #000000;
		border-width: 1rpx;
	}
	.intro {
		margin: 10rpx;
	}
	.oneInput {
		margin-bottom: 50rpx;
		margin-top: 25rpx;
		display: flex;
		flex-direction: row;
	}
	.icons {
		margin-right: 5rpx;
	}
	input {
		
		border-width: 1rpx;
		
		width: 300rpx;
	}
	
</style>
