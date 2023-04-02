<template>
	<view class="page" >
		<u-cell-group >
			
			<u-cell-item 
			 v-for="item in list"
			:key="item.yuyueId"  
			:title="'Vaccination ID:'+item.yuyueId"  
			:label="item.qiandao" 
			:value="'Name :'+item.qiandaoren" 
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
				this.$u.get("/worker/findMyQiandao").then(res=>{
					
						for(let i=0;i<res.data.length;i++){
							
							this.$u.get("/admin/queryUser?id="+res.data[i].qiandaoren).then(result=>{

								res.data[i].qiandaoren=result.data.realName;
								
							})
							//this.list=res.data;
					
							
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
