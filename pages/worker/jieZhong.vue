<template>
	<view class="page">
		<u-button type="primary" @click="scanCode">Scan Code</u-button>
		<view v-show="isShow" class="aaa">
			<view v-if="result.success">
				Yes
			</view>
			<view v-else>
				{{result.message}}
			</view>
		</view>
			<u-form v-show="form.yuyueId" label-width='150' ref="uForm" :model="form">
				<u-form-item label="Inoculation part" prop='jiezhongbuwei'>
					<u-radio-group v-model='form.jiezhongbuwei' >
						<u-radio name='Left upper arm'>Left upper arm</u-radio>
						<u-radio name='Right upper arm'>Right upper arm</u-radio>
					</u-radio-group>
				</u-form-item>
				<u-form-item label="Vaccine batch number" prop='yimiaoPihao'>
					<u-input v-model="form.yimiaoPihao"></u-input>
				</u-form-item>
				<u-form-item label="Vaccine type" prop='yimiaoZhonglei'>
					<u-input v-model="form.yimiaoZhonglei" type='select' @click='show=true'></u-input>
					<u-select v-model='show' :list='list' @confirm='setYimiao'></u-select>
				</u-form-item>
				<u-form-item label="Vaccine production enterprises">
					<u-input v-model="form.yimiaoShengchanqiye" :disabled='true' ></u-input>
				</u-form-item>
				
				
				<u-button type="success" @click="jieZhong">Complete vaccination</u-button>
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
					jiezhongbuwei:'',
					yimiaoId:'',
					yimiaoPihao:'',
					yimiaoShengchanqiye:'',
					yimiaoZhonglei:'',
					chengjishu:''
					
				},
				list:[],
				show:false,
				rules : {
					jiezhongbuwei:[
						
										{ 
											required: true, 
											
											message: 'Please select the vaccination site', 
											
											// 可以单个或者同时写两个触发验证方式 
											trigger: ['blur'],
										},
										
					],
					yimiaoPihao:[
						
										{ 
											required: true, 
											
											message: 'Please enter the vaccine batch number', 
											
											// 可以单个或者同时写两个触发验证方式 
											trigger: ['blur'],
										},
										
					],
					yimiaoZhonglei:[
						
										{ 
											required: true, 
											
											message: 'Please select the type of vaccine', 
											
											// 可以单个或者同时写两个触发验证方式 
											trigger: ['blur'],
										},
										
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
						this.getYimaoList();
						
					}
				})
			},
			getYimaoList(){
				this.$u.get('/yimiao/pageQuery').then(res=>{
					console.log(res)
						this.list=res.data.map(item=>{
							console.log(res)
							return {
								
								label:item.yimiaoShengchanqiye+item.yimiaoZhonglei,
							
								extra:item
							}
						})
					
					
				})				
			},
			jieZhong(){
				this.$refs.uForm.validate(valid => {
						if (valid) {
				this.$u.post("/worker/jiezhong",this.form).then(result=>{
				this.result = result;	
				this.isShow=true;
				this.form={
					yuyueId:'',
					jiezhongbuwei:'',
					yimiaoId:'',
					yimiaoPihao:'',
					yimiaoShengchanqiye:'',
					yimiaoZhonglei:'',
					chengjishu:''
				}
				})
				}
				})
			},
			setYimiao(res){
							this.form.yimiaoId = res[0].extra.id;
							this.form.yimiaoShengchanqiye = res[0].extra.yimiaoShengchanqiye;
							this.form.yimiaoZhonglei = res[0].extra.yimiaoZhonglei;
							this.form.chengjishu = res[0].extra.zhenshu;
						}
			
		}
	}
</script>

<style>
	.aaa{
		font-size: 60px;
	}

</style>
