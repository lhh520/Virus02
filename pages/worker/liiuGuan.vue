<template>
	<view class="page">
		<u-button type="primary" @click="scanCode">扫码</u-button>
		<view v-show="isShow"  class="aaa">
			<view v-if="result.success">
				留观完成
			</view>
			<view v-else>
				{{result.message}}
			</view>
		</view>
			<u-form v-show="form.yuyueId" label-width='150' ref="uForm" :model="form">
				<u-form-item label="时长(分钟)" prop="liiuguanShichang">
					<u-input v-model="form.liiuguanShichang"></u-input>
				</u-form-item>
				<u-form-item label="备注(选填)" prop="liuguanShuoming">
					<u-input type='textarea' v-model="form.liuguanShuoming"></u-input>
				</u-form-item>
		
				<u-button type="success" @click="liuGuan">结束留观</u-button>
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
					liiuguanShichang:'',
					liuguanShuoming:''
					
				},
				rules : {
					liiuguanShichang:[
						
										{ 
											required: true, 
											
											message: '请输入留观时间', 
											
											// 可以单个或者同时写两个触发验证方式 
											trigger: ['blur'],
										},
									
										{
											pattern:/^[2-4]?[0-9]$/,
											message:'时长应在20-50分钟之间',
											trigger: 'blur',
										}
					]
	
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
			liuGuan(){
				
				this.$refs.uForm.validate(valid => {
						if (valid) {	
				this.$u.post("/worker/liuguan",this.form).then(result=>{
				this.result = result;	
				
				this.isShow=true;
				
				this.form={
					yuyueId:'',
					liiuguanShichang:'',
					liuguanShuoming:''
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
