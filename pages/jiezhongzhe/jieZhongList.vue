<template>
	<view class="page">
		<u-cell-group>
				<u-cell-item
				v-for="item of list" 
				:key="item.id"  
				:title="item.yimiaoShengchanqiye+'--'+item.yimiaoZhonglei"  
				:label="item.jiezhongbuwei" 
				:value="item.createTime" 
				icon="order" 
				hover-class="none" 
				:arrow="false"
				></u-cell-item>
				
			</u-cell-group>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[]
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init(){
				this.$u.get("/user/findMyJiezhong").then(res=>{
					console.log(res)
					if(res.success){
						this.list=res.data;
					}
				})
			}
		}
	}
</script>

<style>

</style>
