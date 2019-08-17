<template>
	<view>
	 <view>
		<section >
		  <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
		</section>
	 </view>
	      <view class="contain">
			<view class="about">
				<view class="about-title">
					工作室简介
				</view>
				<view class="about-content">
					{{desc}}
				</view>
				<img  class ="about-img" src="https://image.weilanwl.com/gif/wave.gif" mode = "scaleToFill" alt="">
			</view>
			<view class="video">
				<video class= "video-content" :src='video' controls></video>
			</view>
		<view class="about-text" >
			<view class="card" v-for="(item , index) in infos" :key ="index">
				<img class= "card-header" :src='item.icon'>
				<view class="header-title">{{item.icontitle}}</view>
				<view class="card-body">{{item.body}}</view>
			</view>
		</view>	
		<view class="ziwojieshao">
			<view class="jieshao_title">工作室团队<br/>——————————</view>
				<img src="https://www.jingsoftware.com/chen_xcy/img/context/chenxing.png" alt="" class="jieshao_img" >
					<view class="jieshaoneirong">
						{{jieshao}}<br/>{{jieshaoneirong}}
					</view>
		</view>	
			<view class="jingdiananli">
				<view class="anli-title">经典案例<br/>—————————— </view>
				<img  class="anli-img" src="https://camo.githubusercontent.com/25a919d9549feb0b713ad86472c09e9c3f46aa83/687474703a2f2f696d616765732e70616e64616f6d656e672e636f6d2f32346665333339666662333936636662656238633136306435336261313539342e6a7067" alt="">
				<img src="" alt="">
			</view>
		
		</view>
	</view>
</template>
<script>
	import navigationBar from '../components/navigationBar'
	import {get} from '../../api/homepage.js'

	export default {
	  components: {
		navigationBar,
	  },
	  data() {
		return {
		  videoTitle: '关于瑞宝星',
		  abouts: '',
		  desc:'',
		  video:'',
		  infos:'',
		  jieshao:"陈星",
		  jieshaoneirong:"工作室技术负责人"

		}
	  },
	  methods:{
		  async getAboutInfo() {
			  let _this = this 
			  const res = await get('/api/chen/aboutus',{
				  owner:'chenxing'
			  })
			  _this.abouts = res.aboutus
			  _this.desc= _this.abouts[0].desc
			  _this.video= _this.abouts[0].video
		  },
		  async getCardInfo(){
			  let _this = this
			  const res = await get('/api/chen/jieshao',{
				  owner:'chenxing'
			  })
			  _this.infos = res.jieshao
			  console.log(_this.infos)
			  
		  }
		
	  },
	  onLoad(){
		  this.getAboutInfo()
		  this.getCardInfo()
	   
	  }

};

</script>
<style lang="scss">
	page {
		background: #e7ebed;
	
	}
	
	.about {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-image: url(https://www.jingsoftware.com/chen_xcy/img/context/aboutus.jpg);
		width:100vw;
		height: 300rpx;
		position: relative;
	}
	
	.about-title {
		//border: 1px solid red;
		position: absolute;
		top:15upx;
		font-size: $uni-font-size-lg;
		color:#ffffff;
	}
	
	.about-content{
		position: relative;
		top: -15upx;
		width:90%;
		font-size: $uni-font-size-base;
		color:#ffffff;
	}
	
	.about-img{
		position:absolute;
		width:100%;
		bottom:0;
		z-index: 99;
		height: 100rpx;
		left:0;
		mix-blend-mode: screen;
	}
	
	.video  {
		margin: 5upx auto;
		width:95%;
		height: 250upx;
		border-radius: 8upx;
		display: flex;
		justify-content: center;
		background: #ffffff;
		}
		
	 .video-content {
		 height: 250upx;
		 border-radius: 8upx;
		 overflow: hidden;
	}
	
	.about-text {
		margin:15upx auto;
		width:95%;
		border-radius: 8upx;
		background: #ffffff;
		flex-direction: row;
		display: flex;
		flex-wrap: wrap;
		
	}
	
		.card {
			display: inline-block;
			width:220upx;
			height: 220upx;
			margin: 10upx 60upx 10upx 60upx;
		}
		
		.card-header {
			width:80upx;
			height: 80upx;
			padding: 8upx 8upx;
			margin-left: 45upx;
		}
		
		.header-title {
			height: 40upx;
			width: 140upx;
			margin-left: 20upx;
			text-align: center;
			font-size: $uni-font-size-base;
		}
		
		.card-body {
			height: 70upx;
			width: 250upx;
			font-size:20upx;
		}
		
	  .ziwojieshao {
			@extend .about-text ;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			
	  }
	
	  .jieshao_title {
		  font-size: $uni-font-size-lg;
		 margin: 25upx auto;
		  text-align: center;
		  color:#333333;
	  }
		
	  .jieshao_img {
		  position: relative;
		  width: 100upx;
		  height: 100upx;
		  border-radius: 50upx;
		  bottom:20upx;
	  }
	  
	  .jieshaoneirong {
		  position: relative;
		  font-size: $uni-font-size-sm;
		  text-align: center;
		  color:#333333;
		  bottom:10upx;
	  }
	  
	  .jingdiananli {
		  @extend .ziwojieshao;
	  }
	  
	  .anli-title {
		  font-size: $uni-font-size-lg;
		  margin: 25upx auto;
		   text-align: center;
		   color:#333333;
	  }
	  
	  .anli-img {
		  width : 60upx ;
		  height: 60upx ;
	  }
		

</style>
