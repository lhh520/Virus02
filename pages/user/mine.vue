<template>
	<view class="page">
			<view class='search'>
				<u-avatar :src="src"  mode="circle" ></u-avatar>
			</view>
		<view class="row">
			<view class="label">Name:</view>
			<view class="content">{{userInfo.username}}</view>	
		</view>
		<view class="row">
			<!-- <view class="label">姓&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名:</view> -->
			<view class="label">real_name:</view>
			<view class="content">{{userInfo.realName}}</view>	
		</view>
		<view class="row">
			<view class="label">ID:</view>
			<view class="content">{{userInfo.card}}</view>	
		</view>
		<view class="row" v-show="userType=='jiezhongzhe'">
			<view class="label">Address:</view>
			<view class="content">{{userInfo.address}}</view>	
		</view>
		<view class="row">
			<view class="label">Phone</view>
			<view class="content">{{userInfo.phone}}</view>	
		</view>
		<u-button  type="error" @click="logout" class="logoutBtn">Sign out</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo:{
					"username":"",
					"realName":"",
					"card":"",
					"address":"",
					"phone":"",
	
				},
				src:'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fandroid-artworks.25pp.com%2Ffs01%2F2014%2F11%2F01%2F102_e52d0a2857f5dc15f14d4fdf970effe4.png&refer=http%3A%2F%2Fandroid-artworks.25pp.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1629466197&t=2f6a35af5b3b8f7e9998d323f4dbbf59',
				userType:''
				
			}
		},
		onLoad() {
			this.init()
		},
		methods: {
			logout(){
				this.$store.commit('logout');
				uni.reLaunch({
					url:'login'
				})
				
			},
			init(){
				this.userType=this.$store.state.userLogined.userType;
				this.userInfo=this.$store.state.userLogined.loginUser
			}
		}
	}
</script>

<style scoped>
		.page{
			padding: 50rpx;
		}
		.row{
			display: flex;
			line-height: 150rpx;
			margin-right:50rpx;
			margin-left:50rpx;
		}
		.row+.row{
			border-top: 2rpx solid #eeeeee;
		}
		
		.label{
			width: 120rpx;
		}
		.content{
			flex: 1;
		}
		.content::before{
			content: '';
		}
		.search {
		display:flex;
		justify-content: center;//水平居中
		margin-bottom: 100rpx; 
			}
			.logoutBtn{
				margin-top: 30rpx;
				width: 500rpx;
			}
</style>
