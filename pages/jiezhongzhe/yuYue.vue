<template>
	<view class="page">
		<u-form :model="form" ref="uForm" label-width='150'>
				<u-form-item label="Site">
					<u-input type='select' v-model="form.jiezhongdianName" @click="show=true"/>
					<u-select v-model='show' :list='list' @confirm='setJieZhongDian'></u-select>
				</u-form-item>
				<u-form-item label="Date">
					<u-input type='select' v-model="form.yuyueriqi" @click="show2=true"/>
					<u-calendar v-model="show2"  @change="setDate" :min-date="$u.timeFormat(new Date(),'yyyy-mm-dd')" :max-date="$u.timeFormat(new Date().setDate(new Date().getDate()+7),'yyyy-mm-dd')" ></u-calendar>
				</u-form-item>
				<u-button :loading="isLoading" @click="yuYue" type="success">Vaccinate</u-button>
		</u-form>
		<u-toast ref="uToast" />
				
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form:{
					yuyueriqi:'',
					jiezhongdianId:'',
					jiezhongdianName:'',
					jiezhongdianAddress:''
				},
				show:false,
				show2:false,
				list:[],
				isLoading:false
			}
		},
		onLoad() {
			uni.setLocale('en');
			this.init();
		},
		methods: {
			init(){
				uni.setLocale('en');
				this.$u.get('/jiezhongdian/findAll').then(res=>{
					console.log(res)
					if(res.success){
						this.list = res.data.map(function(item){
							return {
								label:item.name,
								value:item.id,
							
							}
						})
					}
				})
			},
			setJieZhongDian(res){
				uni.setLocale('en');
				this.form.jiezhongdianId =res[0].value;
				this.form.jiezhongdianName =res[0].label;
			},
			setDate(res){
				this.form.yuyueriqi=res.result;
			},
			yuYue(){
				this.isLoading=true;
				this.$u.post("/user/yuyue",this.form).then(res=>{
					if(res.success){
						uni.showModal({
						    title: 'Appointment successful',
						    content: 'You have successfully made an appointment! Please arrive at the scheduled vaccination site on time to complete the appointment',
							showCancel:false,
						    success(res) {
								if (res.confirm) {
									uni.navigateBack();
								}
							}
							})
					}else{
						this.$refs.uToast.show({
							title :res.message,
							type : 'error'
						})
					}
				}).finally(()=>{
					this.isLoading=false;
				})
			}
			
		}
	}
</script>

<style scoped>
.page{
		padding: 50rpx;
		
	}
</style>
