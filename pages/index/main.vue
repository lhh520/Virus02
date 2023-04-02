<template>
	<view class="page">
		<!-- 这是接种者页面 -->
		<view v-if="userType == 'jiezhongzhe'">
			<u-cell-group>
				<u-cell-item @click="goto('yuYue')" icon='pushpin' title="Appointment" label="Make an appointment"></u-cell-item>
				<u-cell-item @click="goto('yuYueList')" icon='order' title="Appointment history" label="Appointment history"></u-cell-item>
				<u-cell-item @click="goto('jieZhong')" icon='checkmark-circle' title="Vaccinate" label="Arrive the site and start Vaccination"></u-cell-item>
				<u-cell-item @click="goto('jieZhongList')" icon='list-dot' title="Vaccination history" label="View vaccination history"></u-cell-item>
			</u-cell-group>
		</view>
			
		
		<!-- 这是医护工作者 -->
		<view v-else>
			<u-cell-group>
				<u-cell-item @click="goto2('qianDao')" icon='pushpin' title="Check in" label="Check in before vaccination"></u-cell-item>
				<u-cell-item @click="goto2('yuJian')" icon='order' title="Pre Inspection" label="Pre Inspection"></u-cell-item>
				<u-cell-item @click="goto2('jieZhong')" icon='checkmark-circle' title="Vaccinate" label="Arrive at the site and start vaccination"></u-cell-item>
				<u-cell-item @click="goto2('liiuGuan')" icon='list-dot' title="Observation" label="keep in the hospital observation"></u-cell-item>
			</u-cell-group>
			
			<u-line/>
			
			<u-cell-group>
				<u-cell-item @click="goto2('qianDaoList')" icon='pushpin' title="Check in history" label="Check in before vaccination"></u-cell-item>
				<u-cell-item @click="goto2('yuJianList')" icon='order' title="Pre Inspection history" label="View Pre Inspection history"></u-cell-item>
				<u-cell-item @click="goto2('jieZhongList')" icon='checkmark-circle' title="Vaccination history" label="View Vaccination history"></u-cell-item>
				<u-cell-item @click="goto2('liuGuanList')" icon='list-dot' title="Observation history" label="View Observation history"></u-cell-item>
			</u-cell-group>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userType:''
			}
		},
		onLoad(){
			this.init();
		},
		methods: {
			init(){
			
				this.userType=this.$store.state.userLogined.userType;
				console.log(this.userType)
			},
			goto(res){
				uni.navigateTo({
					url:'../jiezhongzhe/'+res,
				})
			},
			
			goto2(res){
				uni.navigateTo({
					url:'../worker/'+res,
				})
			}
			
		}
	}
</script>

<style scoped>
		
</style>
