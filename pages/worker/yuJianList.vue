<template>
	<view>
	
		<u-cell-group>
			
			<u-cell-item 
			 v-for="item of list"
			:key="item.yuyueId"  
			:title="'预约ID:'+item.yuyueId+' - 接种人:'+item.jiezhongren"  
			:label="item.createTime" 
			:value="'高压:'+item.gaoya+' 低压:'+item.diya+' 体温:'+item.tiwen" 
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
				list:[],
				limit:6,
				page:1
			}
		},
		onLoad(){
			this.init();
		},
		methods: {
			init(){
				this.$u.get("/worker/findMyYujian").then(res=>{
					
						for(let i=0;i<res.data.length;i++){
							
							this.$u.get("/admin/queryUser?id="+res.data[i].jiezhongren).then(result=>{

								res.data[i].jiezhongren=result.data.realName;
								
							})
				
						}
		
						this.list=res.data;
						console.log(this.list);
	
				})
			}
	
		}
	}
</script>

<style>

</style>
