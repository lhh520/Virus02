<template>
	<view class="page">
		<u-form label-width="200rpx" :model="form" ref="uForm" class="login">
			
			<u-form-item label="Username">
				<u-input v-model="form.username" />
			</u-form-item>
			
			<u-form-item label="Password">
				<u-input type="password" v-model="form.password" />
			</u-form-item>
			
			<u-form-item label="Role">
				<u-input type="select" @click="userTypeShow=true"  v-model="form.userTypeName" />
				<u-select @confirm="userTypeConfirm" v-model="userTypeShow" :list="userType"></u-select>
			</u-form-item>
			<u-button :loading="isLoading" @click="login" type="success" class="loginBtn">Sign in</u-button>
		
		</u-form>
				<!-- <view class="tips">
					<text @click="toReg">注册</text>
				</view> -->
				<u-button  @click="toReg()" type="info" class="regBtn">Sign up</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form:{
					username:'',
					password:'',
					userType:'jiezhongzhe',
					userTypeName:'Vaccinator'
				},
				userType:[
					{label:'Vaccinator',value:'jiezhongzhe'},
					{label:'Doctor',value:'worker'}
				],
				userTypeShow:false,
				isLoading:false
			}
		},
		methods: {
			userTypeConfirm(res){
				this.form.userTypeName=res[0].label;
				this.form.userType=res[0].value;
					// console.log(this.form)
			},
			login(){
				this.isLoading=true;
				const _this=this;
				this.$u.post("/login",this.form).then((res)=>{
					if(res.success){
						
						//登录成功:信息写入数据仓库及硬盘中,并且跳转到首页
						//1.将数据写入数据仓库
						this.$store.commit('login',res.data);
						uni.switchTab({
							url:"../index/main"
						})
					}else{
						
						uni.showToast({
							title:res.message
						})
					}
				}).finally(()=>{
					this.isLoading=false;
				})
				
				
			},
			toReg(){
				uni.reLaunch({
					url:"reg"
				})
			}
	
			
		}
	}
</script>
<style>
	page{
		
		background-image: linear-gradient(#ffffff,#c8c5b3 );
	}
</style>

<style scoped>
			
			.tips{
				
				text-align: right;
				margin-top: 20rpx;
			}
			.tips text{
				color: blue;
				line-height: 3em;
			}
			.login{
					margin-top:180rpx;
					margin-bottom:40rpx;
					margin-right:50rpx;
					margin-left:50rpx;
			}
			
			.regBtn{
				
				color:#18B566;
				width: 550rpx;
			}
			
</style>
