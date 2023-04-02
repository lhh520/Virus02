<template>
	<view class="page">
		<u-button type="primary" @click="scanCode">扫码</u-button>
		<view v-show="isShow" class="aaa">
			<view v-if="result.success">
				签到成功
			</view>
			<view v-else>
				{{result.message}}
			</view>
		</view>
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				result:{},
				isShow:false
			}
		},
		methods: {
			scanCode(){
				this.isShow=false;
				uni.scanCode({
					onlyFromCamera: false,
					scanType: ['qrCode'],
					success: res=>{
						const yuyueId=JSON.parse(res.result).id;
						this.$u.post("/worker/qiandao",{yuyueId}).then(result=>{
						this.result = result;	
						console.log(result)
						this.isShow=true;
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
.page{
		padding: 50rpx;
		
	}
</style>
