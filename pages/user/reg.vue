<template>
	<view class="page">
		<u-form label-width="200rpx" :model="form" ref="uForm" class="reg">
			
			<u-form-item label="username" prop="username">
				<u-input v-model="form.username" />
			</u-form-item>
			
			<u-form-item label="username" prop="password">
				<u-input type="password" v-model="form.password" />
			</u-form-item>
			
			<u-form-item label="password2" prop="password2">
				<u-input type="password" v-model="form.password2" />
			</u-form-item>
			
			<u-form-item label="realName" prop="realName">
				<u-input v-model="form.realName" />
			</u-form-item>
			
			<u-form-item label="card" prop="card">
				<u-input v-model="form.card" />
			</u-form-item>
			
			<u-form-item label="phone" prop="phone">
				<u-input v-model="form.phone" />
			</u-form-item>
			
			<u-form-item label="areaName" prop="areaName">
				<u-input v-model="form.areaName" type="select" @click="isShow=true" />
				<u-picker v-model="isShow" mode="region" @confirm="setArea"></u-picker>
			</u-form-item>
			
			<u-form-item label="address" prop="address">
				<u-input v-model="form.address" />
			</u-form-item>
			
			
			<u-button :loading="isLoading" @click="reg" type="success">注册</u-button>
		<!-- <view class="tips">
			<text @click="toLogin">返回</text>
		</view> -->
		</u-form>
		<u-button  @click="toLogin"  class="back">返回</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
					
				form:{
					username:'',
					password:'',
					password2:'',
					realName:'',
					card:'',
					phone:'',
					area:'',
					areaName:'',
					address:''
		
				},
				isLoading:false,
				isShow:false,
				rules:{
					username:[
						{
						required: true, 
						message: 'enter user name', 
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change','blur'],
						
						},
						{
							min: 3,
							message:'At least 3 characters',
							trigger: ['change','blur'],
						},
						{
							max: 18,
							message:'Up to 18 characters',
							trigger: ['change','blur'],
						}
					],
					password:[
						{
							min:6,
							max:20,
							message:' between 6 and 20 digits',
							trigger:'blur'
						}
					],
					password2:[
						{
							min:6,
							max:20,
							message:'between 6 and 20 digits',
							trigger:'blur'
						},
						{
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// this.$u.test.mobile()就是返回true或者false的
								return this.form.password==this.form.password2;
							},
							message: 'Two different passwords',
							// 触发器可以同时用blur和change
							trigger: 'blur',
						}
					],
					realName:[
						{
						required: true, 
						message: 'Please enter your name', 
						// 可以单个或者同时写两个触发验证方式 
						trigger: ['change','blur'],
						
						},
						{
							min: 2,
							message:'At least 2 characters long',
							trigger: ['change','blur'],
						},
						{
							max: 10,
							message:'At least 10 characters long',
							trigger: ['change','blur'],
						}
					],
					card:[
						
						{
							pattern:/^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/,
						message:'Incorrect format of ID card',
						trigger: ['change','blur'],
						}
					],
					phone:[
						{
							pattern: /^(13[0-9]|14[01456879]|15[0-35-9]|16[2567]|17[0-8]|18[0-9]|19[0-35-9])\d{8}$/,
							message:'Incorrect phone number format',
							trigger: ['change','blur'],
						}
					],
					address:[
						{
							required: true,
							message: 'Please enter the address', 
							// 可以单个或者同时写两个触发验证方式 
							trigger: ['change','blur'],
						}
					]
					
				}
			}
		},
			onReady() {
				this.$refs.uForm.setRules(this.rules);
			},
		methods: {
			reg(){
				this.$refs.uForm.validate(valid => {
								if (valid) {
									this.isLoading=true;
									this.$u.post("/user/register",this.form).then((res)=>{
										if(res.success){
											uni.showModal({
											    title: 'login was successful',
											    content: 'You have successfully registered! Click OK to jump to the login interface',
												showCancel:false,
											    success: function (res) {
											        if (res.confirm) {
														uni.reLaunch({
															url:'login'
														})
											            console.log('用户点击确定');
											        } else if (res.cancel) {
											            console.log('用户点击取消');
											        }
											    }
											});
										}else{
											uni.showToast({
												title:res.message
											})
										}
									}).finilly(()=>{
									this.isLoading=false;	
									})
								} 
							});
			},
			setArea(res){
				
				this.form.area=res.area.value;
				this.form.areaName=res.province.label+res.city.label+res.area.label
			},
			toLogin(){
				uni.reLaunch({
					url:'login'
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
	.page{
		padding: 80rpx;
	}
	.tips{
		
		text-align: right;
		margin-top: 20rpx;
	}
	.tips text{
		color: blue;
		line-height: 3em;
	}
	.reg{
		margin-top:10rpx;
		margin-bottom:40rpx;
		margin-right:50rpx;
		margin-left:50rpx;
	}
	.back{
		color:#18B566;
		width: 500rpx;
	}
	

</style>
