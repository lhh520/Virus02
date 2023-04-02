<template>
	<view>
		<u-cell-group>
			
			<u-cell-item 
			v-for="item of list"
			:key="item.yuyueId"  
			:value="'Vaccine name：'+item.yimiaoShengchanqiye+' - '+item.yimiaoZhonglei"  
			:label="item.createTime" 
			:title="'ID'+item.yuyueId+' -  Name:'+item.jiezhongren" 
			icon="order" 
			hover-class="none" 
			:arrow="false"
			
			>
			
			</u-cell-item>

		</u-cell-group>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// data:{
				// 	jiezhongbuwei:'',
				// 	jiezhongren:'',
				// 	yimiaoZhonglei:''

				// },
				
				list:[
					{
						
					}
				],
				limit:6,
				page:1
			}
		},
		onLoad(){
			this.init();
		},
		methods: {
			init(){
				this.$u.get("/worker/findMyJiezhong").then(res=>{
					
					if(res.success){
						for(let i=0;i<res.data.length;i++){
						this.$u.get("/admin/queryUser?id="+res.data[i].jiezhongren).then(result=>{
						
							res.data[i].jiezhongren=result.data.realName;
							
						})
						
						}
						this.list=res.data
						
					}
				})
			}
			
			
			
		}
	}
</script>

<style>

</style>
