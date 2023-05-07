<template>
	<view class="page">
		<u-button type="primary" @click="scanCode">Scan Code</u-button>
		<view v-show="isShow"  class="aaa">
			<view v-if="result.success">
				Success
			</view>
			<view v-else>
				{{result.message}}
			</view>
		</view>
			<u-form v-show="form.yuyueId" label-width='150' :model="form" ref="uForm">
				<u-form-item label="low pressure" prop="diya">
					<u-input v-model="form.diya"></u-input>
				</u-form-item>
				<u-form-item label="high pressure" prop="gaoya">
					<u-input v-model="form.gaoya"></u-input>
				</u-form-item>
				<u-form-item label="temperature" prop="tiwen">
					<u-input v-model="form.tiwen"></u-input>
				</u-form-item>
				<u-form-item label="Medication or not">
					<u-radio-group v-model="form.isFuyao">
						<u-radio :name='0'>No</u-radio>
						<u-radio :name='1'>Yes</u-radio>
					</u-radio-group>
					<u-input v-model="form.isFuyao"></u-input>
				</u-form-item>
				<u-form-item label="Drug name" v-show="form.isFuyao==1">
					<u-input v-model="form.yaowumingcheng"></u-input>
				</u-form-item>
				<u-button type="success" @click="yujian">Pre inspection</u-button>
			</u-form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				result:{},
				isShow:false,
				form:{
					yuyueId:'',
					diya:'',
					gaoya:'',
					isFuyao:0,
					tiwen:'',
					yaowumingcheng:''
				},
				rules : {
					diya:[
						
										{ 
											required: true, 
											message: '请输入血压', 
											// 可以单个或者同时写两个触发验证方式 
											trigger:'blur',
										},
										{
											pattern:/^[6-8]?[0-9]$/,
											message:'低压范围在60-90之间，如果不在此区间，建议咨询医生后在接种',
											trigger: 'blur',
										}
										
					],
					gaoya:[
									{ 
										required: true, 
										message: '请输入血压', 
										// 可以单个或者同时写两个触发验证方式 
										trigger: 'blur',
									},
									{
										pattern:/^9[0-9]|1[0-5][0-9]$/,
										message:'低压范围在90-160之间，如果不在此区间，建议咨询医生后在接种',
										trigger: ['change','blur'],
									}
								
					],
					tiwen:[
									{ 
										required: true, 
										message: '请输入体温', 
										// 可以单个或者同时写两个触发验证方式 
										trigger: ['blur'],
									},
													
									{
											pattern:/^3[4-7]\.?[0-9]$/,
											message:'您的体温异常，建议咨询医生后在接种',
											trigger: ['change','blur'],
										},
										
					],
					
					
				}
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		methods: {
			scanCode(){
				this.isShow=false;
				uni.scanCode({
					onlyFromCamera: false,
					scanType: ['qrCode'],
					success: res=>{
						this.form.yuyueId=JSON.parse(res.result).id;
	
					}
				})
			},
			yujian(){
				this.$refs.uForm.validate(valid => {
						if (valid) {				
								this.$u.post("/worker/yujian",this.form).then(result=>{
								this.result = result;	
								
								this.isShow=true;
								
								this.form={
									yuyueId:'',
									diya:'',
									gaoya:'',
									isFuyao:0,
									tiwen:'',
									yaowumingcheng:''
								}
								})
						}
					})
				}
			}
	}
</script>

<style>
	.aaa{
		font-size: 60px;
	}

</style>
