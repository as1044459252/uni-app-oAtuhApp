<template>
	<view class="container">
		<view class="container">
			<text class="intro">这是一个基于Oauth2.0，uni-app，shiro，springboot的项目</text>
			<form @submit="register" @reset="">
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
				<button form-type="submit">注册</button>
			</form>
		</view>
		<navigator url="login">
		<button>去登录</button>
		</navigator>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				href: 'https://uniapp.dcloud.io/component/README?id=uniui'
			}
		},
		methods: {
			register(e){			
				uni.request({
					url: 'http://localhost:8081/register',
					method: 'POST',
					data: {
						'username':e.detail.value.username,
						'password':e.detail.value.password,
						},
					header:{
						'content-type':'application/json'
					},
					success: (res) => {
						console.log(e.detail.value.username);
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
		margin-top: 0rpx;
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
