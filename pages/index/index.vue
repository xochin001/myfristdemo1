<template>
<view>
	<view>
		<section >
	      <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
	    </section>
	</view>
	<view class="container">
		<view class="titleNview-background">
			<swiper class="carousel"
			 :indicator-dots="true" 
			 :autoplay="true"
			 :interval="6000"
			 :duration="1000"
			 :circular="true"
			 >
			 <swiper-item class="carousel-item" v-for="(item,index) in imgs" :key="index">
				 <image  mode="aspectFill" :src="item.img" />
			 </swiper-item>
			</swiper>
		</view>
	 <!-- 分类 -->
	<view class="tabs">
	    <scroll-view id="tab-bar" class="scroll-h" :scroll-x="true" :show-scrollbar="false" :scroll-into-view="scrollInto" :id="fixtop?'fix' :''">
	        <view v-for="(tab,index2) in tabBars" :key="tab.id" class="uni-tab-item" :id="tab.id" :data-current="index2" @click="ontabtap">
				<img class="imgs" :src='tab.img' alt="">
				<div class="uni-tab-item-title" :class="tabIndex==index2 ? 'uni-tab-item-title-active' : ''">{{tab.name}}</div>
	        </view>
	    </scroll-view>
	</view>
	    <view class="context">
			<homepage v-if="tabIndex===0"/>
			<lock v-if="tabIndex===1"/>
			<interactive v-if="tabIndex===2"/>
		</view>
	</view>
</view>
</template>

<script>
	import navigationBar from '../components/navigationBar.vue'
	import {get} from '../../api/homepage.js'
	import homepage from '@/pages/detail/homepage.vue'
	import lock from '@/pages/detail/lock.vue'
	import interactive from '@/pages/detail/interactive.vue'
	export default {
		components:{
			navigationBar,
			homepage,
			lock,
			interactive
		},
		data() {
			return {
			  navBarHeight: 0,
			  fixtop: false,
			  videoTitle: '瑞宝星微程序开发',
			  imgs:'',
			  tabIndex: 0,
			  scrollInto: "",
			  tabIndex:0 ,
			  tabBars:[{
				  name:'主页',
				  id:'zhuye',
				  img:"/static/zhuye.png",
				},{
				  name:'方案',
				  id:'fangan',
				  img:"/static/fangan.png",
				},{
				  name:'动态',
				  id:'dongtai',
				  img:"/static/news.png",
			  }]
			  
			}
		},
		  beforeMount() {
		    const self = this
		    wx.getSystemInfo({
		      success(system) {
		        console.log(`system:`, system)
		        self.statusBarHeight = system.statusBarHeight
		        let platformReg = /ios/i
		        if (platformReg.test(system.platform)) {
		          self.titleBarHeight = 44
		        } else {
		          self.titleBarHeight = 48
		        }
		        self.navBarHeight = self.statusBarHeight + self.titleBarHeight
				console.log('?'+self.navBarHeight)
		      }
		    })
		  },
		
		onLoad() {
			this.getimgs()
			let that = this
			    const query = wx.createSelectorQuery()
			    query.select(".scroll-h",".uni-tab-item").boundingClientRect()
			    query.exec(function(res) {
			      that.top = res[0].top -that.navBarHeight
			    })
			  },
			   onPageScroll: function(e) {
			      var that = this
			      console.log(that.top - that.navBarHeight)
			      if (e.scrollTop >= that.top) {
			        that.fixtop = true
			      } else {
			        that.fixtop = false
			    }
		},
		methods: {
			async getimgs() {
				const res = await get('/api/chen/banners',{
					owner: 'chenxing'
				})
				this.imgs = res.banner
			},
			ontabtap(e) {
			    let index = e.target.dataset.current || e.currentTarget.dataset.current;
				this.tabIndex = index
			},
		}
	}
</script>

<style lang="scss">
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		
	}
	page {
		background: #e7ebed;
	
	}
	
	.titleNview-background {
		margin: 0 auto;
		top: 0;
		left: 0;
		width: 100%;
		height: 350upx;
		transition: .4s;
	}
	.carousel {
		width: 100%;
		height: 350upx;
	
		.carousel-item {
			width: 100%;
			height: 100%;
			overflow: hidden;
		}
	
		image {
			width: 100%;
			height: 100%;
			text-align: center;
		}
	}
	
	.tabs {
	    flex-direction: column;
	    overflow: hidden;
	    background-color: #ffffff;
	}
		
	.scroll-h {
	    width: 750upx;
	    height: 150upx;
		white-space: nowrap;
		text-align:center;
		background: #ffffff;
	}	
	.uni-tab-item {
			display: inline-block;
			height: 120upx;
			width:150upx;
			margin: 25upx auto;
		    padding-left: 34upx;
		    padding-right: 34upx;
		}

	.imgs{
		height: 85upx;
		width:85upx;
	}
			
	.uni-tab-item-title {
	    color: #555;
	    font-size: $uni-font-size-sm ;
	    flex-wrap: nowrap;
	    /* #ifndef APP-PLUS */
	    white-space: nowrap;
	    /* #endif */
	}
	
	.uni-tab-item-title-active {
	    color: #007AFF;
	}
		
	#fix {
		  position: fixed;
		  top:  127upx;
		  left: 0;
		  width: 100%;
		  z-index: 10;
		  animation: move 0.2s linear;
	}
	.context{
		border: 1px solid red;
		 width:95%;
		 margin: 20upx auto;
		 border-radius: 10upx;  
	}
	
	
	
	
	

	
</style>
