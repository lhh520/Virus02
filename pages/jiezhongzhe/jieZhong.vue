<template>
	<view class="page">
		<view v-if="qrCodePath">
			<u-image width="650rpx" height="650rpx" :src="qrCodePath"></u-image>
			<view>将此二维码展示给疫苗接种点工作人员</view>
		</view>
		<view v-else>
			<u-empty text="暂无预约" mode="search"></u-empty>

		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				qrCodePath:''
				
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init(){
				this.$u.get("/user/findMyYuyues").then(res=>{
					if(res.success){
						
						for(let i=0;i<res.data.length;i++){
							
							if(res.data[i].status==0){
								this.qrCodePath = this.$u.http.config.baseUrl + res.data[i].qrCodePath;
								console.log(this.qrCodePath);
							break;
							}
						}
					}
				})
			}
		}
	}
</script scoped>


<style>

</style>
