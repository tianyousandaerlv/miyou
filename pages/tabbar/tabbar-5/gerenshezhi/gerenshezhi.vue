<template>
	<view>
		
		<!-- 一般用法 -->
		<uni-list>
		    
			<uni-list-item title="账号" :showArrow="false" :rightText="account"></uni-list-item>
		
		
			<uni-list-item title="手机号" :showArrow="false" :rightText="mobile ? mobile : '未绑定手机号'"></uni-list-item>
			
			<navigator url="./mimashezhi/mimashezhi" open-type="navigate" hover-class="">
				<uni-list-item title="密码设置" :showArrow="true"></uni-list-item>
			</navigator>
		</uni-list>
		<uni-list class="listBorderTop">
			<uni-list-item title="接受新消息通知" :show-switch="true" :showArrow="false" :switchChecked="isSwitch"></uni-list-item>
		</uni-list>
		
		<button @click="quitLogin" style="background-color:#4cb964; position:absolute; bottom: 50upx; width: 690upx; left:30upx;" type="primary">退出登录</button>
	</view>
</template>

<script>
	import uniList from "@/components/uni-list/uni-list.vue"
	import uniListItem from "@/components/uni-list-item/uni-list-item.vue"
	
	
	export default {
		components: {uniList,uniListItem},
		data() {
			return {
				account: '',
				mobile: "",
				isNewMessage: true,
				isSwitch: true
				
			}
		},
		onLoad(option) {
			this.userInfo = JSON.parse(uni.getStorageSync('userInfo')).user
			this.account = this.userInfo.nickName
			this.mobile = this.userInfo.phone
		},
		
		created() {  
			
		},  
		methods: {
			quitLogin(){
				uni.showModal({
					title: "提示",
					content: '是否退出登录',
					success:  (res)=> {
						if (res.confirm) {
							try{
								uni.removeStorageSync('userInfo') //清空token
								uni.$off('redUpdate')
								uni.$off('updateInfo')
								uni.$off('update')
								uni.$off('updateWs')
								uni.$off('updateUserInfo')
								uni.$off('fahongbao')
							}catch(err){
								console.log('处理退出时出错',err)
							}
							
							uni.reLaunch({
							    url: '../../../login/login'
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
						
						
					}
				})
			}
		}
	}
</script>

<style>
	page {
		background-color: #eee;
		
	}
	.uni-list-item__extra span{
		font-size: 14px;
	}
	
	
	.uni-list-item .uni-list-item__container::after{
		/* background-color: #eee!important; */
	}
	.uni-list-item:nth-of-type(2) .uni-list-item__container::after{
		display: none;
	}
	.listBorderTop .uni-switch-input-checked{
		background-color: #4cb964!important;
		border-color: #4cb964!important;
	}
</style>
