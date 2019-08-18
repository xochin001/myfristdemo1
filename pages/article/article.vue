<template>
	<view>
	<view class="content">
		<u-parse :content="context"/>
		<!-- 点赞框 -->
		  <view class ="dianzan"  >
			  <p> 全部点赞:{{dianzans.length}}</p>
			  <view class="box" >
			   <!--  <p v-if="dianzhan==true">{{nickname}}</p> -->
				<view v-if='dianzans.length >= 1' class="dianzanbox"  v-for="(item , index) in dianzans" :key = "index" >
				 <!--  <img class="dianzanimg":src=" item.avator" alt=""> -->
				  <p>{{item.username}},</p>
				</view>
			 </view>
		  </view>
		  <!-- 评论框 -->
			  <scroll-view scroll-y="true" class ="pinglun" >
					<p class="title">所有评论</p>
					<p class="blank" v-if='pingluns.length===0' >不如添加一条评论?</p>
			    <view  v-if='pingluns.length >= 1' class = "pinglun_detail" v-for="(item , keys) in pingluns" :key = "keys"> 
					<view class="tou">
						<img :src="item.avator" alt="">
							<p class= "d1">{{item.username}}</p>
							<p class = "d2">{{times[keys]}}</p>
					</view>
				  <view class="box">
					   {{item.content}}
				 </view>
			 </view>
		  </scroll-view>
	</view>
	<!-- tabbar评论框体 -->
		<view class ="bottom_bar" >
			 <view class="bar_tubiao" @click="add_dianzan()">
				<view class="icon"  >
					  <view class="dianzan" :class="[dianzhanFlag?'active': '']"> 
					  </view>
				</view>
				   <p>点赞</p>
			</view>
				<view class="bar_tubiao" @click="to_pinglun(articleid )">
					<view class="icon1" >
						<span>{{pingluns.length}}</span>
							<img src="/static/images/pinglun.png" alt="">
					</view>
						<p>评论</p>
					</view>
			   <view class="bar_tubiao">
					<view class="icon" >
						<view class="img shoucang"> 
						</view>
					</view>
						<p>收藏</p>
				</view>
			<view class="bar_fabiaopinglun" @click="to_pinglun(articleid )">
				<p>发表评论</p>
			</view>
		</view>
	</view>
</template>
<script>
import { formatTime} from '../../common/utils/index'
import { login ,get ,post } from '../../api/homepage'
import uParse from '@/components/u-parse/u-parse.vue'

export default {
  created(){
    if (login()) {
      this.userInfo = login()
      this.nickname = this.userInfo.nickName
      this.avator = this.userInfo.avatarUrl
      //在最开始的时候给一个初始值，判断flag
      wx.setStorageSync('dianzhanFlag',false)
      wx.setStorageSync('dianzhan',false)
    }

  },

    onShow() {
    this.id = this.$root.$mp.query.id
    this.getaritcle()
    this.get_pinglun()  
    //this.add_dianzan() 
    this.get_dianzan()
  
  },


  data() {
    return {
      avator:'',
      articles:[],
      title:'',
      articleid: 1,
      context :'',
      dianzan:0 ,
      times:[],
      pingluns:{},
      allnumber:0,
      dianzhanFlag:false,
      dianzhan:false,
      dianzans:{},
      dianzanuser:[],
      click:0,
      nickname: ''


    }
  },
   components: {
	uParse
  },
  methods: {


    async getaritcle () {
       
      const res = await get('/api/chen/article_detail' , {
        article_id: this.id
      })
      this.articles = res.data
      this.articleid = this.articles[0].article_id
      this.context= this.articles[0].context
      this.title= this.articles[0].title
      this.dianzan = this.articles[0].dianzan
      this.click = this.articles[0].click
       wx.setNavigationBarTitle({
           title: this.title,
         })
    },
    to_pinglun(id){
      if(login()){
      wx.navigateTo({
            url:"../comment/main?id=" +id 
          })
    }else{
          wx.showToast({
              title: "请先登陆",
              icon:"none",
              duration: 1500,
              mask: true,
            })
           setTimeout(function(){wx.navigateTo({
            url:"/pages/login/main"
          })
          },1500)
         

      }
    },
    async get_pinglun() {
       
      
       const res = await get('/api/chen/pinglun_detail' , {
        pinglun_id : this.id
       })
        this.pingluns = res.data
       
            for( let i = 0 ; i < this.pingluns.length ; i ++) {
              var time = formatTime(new Date(this.pingluns[i].create_time))
              this.times[i] = time
            }
    },
    async post_dianzan( ){

      if(login()){

        const flag = wx.getStorageSync('dianzhanFlag')
        if(flag){
          if(this.dianzanuser.length===0 ||this.dianzanuser.indexOf(this.userInfo.nickName) < 0){
         const res = await post('/api/chen/dianzan' , {
         article_id : this.articleid,
         username : this.userInfo.nickName,
         avator: this.userInfo.avatarUrl
        })
       }
      }
     }else return

    },
    add_dianzan() {
       if(login()){
        this.dianzhanFlag = !this.dianzhanFlag
      if(this.dianzhanFlag){
       wx.setStorageSync('dianzhanFlag',true)
        // if(this.dianzanuser.indexOf(this.userInfo.nickName) < 0){
        //   wx.setStorageSync('dianzhan',true)
        // }
      }else{
        wx.setStorageSync('dianzhanFlag',false);
      }
    }else{
          wx.showToast({
              title: "请先登陆",
              icon:"none",
              duration: 1500,
              mask: true,
            })
           setTimeout(function(){wx.navigateTo({
            url:"/pages/login/main"
          })
          },1500)
      }
    },
    async get_dianzan( ){
        const res = await get('/api/chen/dianzan_detail', {
          article_id : this.id
        })
        this.dianzans= res.data
             for( let i = 0 ; i < this.dianzans.length ; i ++) {
              this.dianzanuser[i] = this.dianzans[i].username
            }

    },
    async liulanliang(){
      const res = await post('/api/chen/article_liulanliang',{
        id: this.id,
        click: this.click +1
      })


        
      
    }
   
  },
  computed: {

  },
  onUnload() {
    this.post_dianzan()
    this.liulanliang()
  }

};

</script>
<style lang="scss">
@import url("../../components/u-parse/u-parse.css");
  .content {
      width: 95vw;
      height: auto;
      box-sizing: border-box;
      justify-content: center;
      padding-top: 1rpx;
      background-color: #e7ebed;
      .articles{
        width:100%;

      }
    .dianzan {
      width: 100%;
      height:100px;
      border-radius:3px;
      background-color:#ffffff;
      margin: 30px auto ;
      border-radius:3px;
    p {
          font-family:"微软雅黑";
          font-size:12px;
          width:auto ;
          height:20px;
           border-bottom: 1px solid #e7ebed;
          }
    .box {
         height:85px ;
         width: 300px;
         padding-top:5px;
          display:flex;
          flex-direction:cow;
    .dianzanbox  {
          width: 40px;
          margin-left:2px;
        p{
          font-family:"微软雅黑";
          font-size:11px;
        }
    .dianzanimg{
        height:44rpx;
        width:44rpx;
        border-radius: 50px;
      }
     
      }
    }
    }
    .pinglun {
      
      width: 100%;
      height:350px;
      border-radius:3px;
      background-color:#ffffff;
      
      margin: 10px auto ;
     .title {
          font-family:"微软雅黑";
          font-size:12px;
          width:auto ;
          height:20px;
           border-bottom: 1px solid #e7ebed;
        }
      .pinglun_detail{
         height:auto ;
         padding-top:5px;
         display:flex;
         flex-direction:column;
         border-bottom: 1px solid #e7ebed;
         .tou{
          width:100%;
          display:flex;
         flex-direction:cow;
         img{
          float:left;
          margin-left:10px;
          top:5px ;
          width:25px;
          height:25px;
          border-radius:50px;    
         }
        .d1 {
          margin-left:5px;
          margin-top:5px;
          height:12px;
          width:40px;
          font-family:"微软雅黑";
          font-size:12px;
              }
        .d2{
          margin-left:5px;
          margin-top:5px;
          height:11px;
          width:80px;
         font-family:"微软雅黑";
          font-size:8px;
        }
           }
        .box {
          
          font-family:"微软雅黑";
          font-size:13px;
          padding-top:10px;
          padding-left:10px;
          }
        }
      .blank{
          font-family:"微软雅黑";
          font-size:13px;
          padding-top:10px;
          padding-left:10px;
      }
      
    }


    


  }
    .bottom_bar{
      background-color:#ffffff;
      padding: 0;
      height: 60px;
      border: 1px solid #e7ebed;
      position:fixed;
      width:100%;
      left:0;
      bottom:0 ;
      z-index: 9999;
      display:flex;
      justify-content:flex-start;
      box-shadow: 0 -1rpx 6rpx rgba(0, 0, 0, 0.1);
    .bar_tubiao{
      height: 60px;
      width: 50px;
      flex-direction:column;
      display:flex;
      margin:0px 10px 0px 10px;
      .icon1{
         position: relative;
        height: 60rpx;
       margin-left:4px;
        width: 60rpx;
         span {
          height: 28rpx;
          width: 28rpx;
          z-index: 10;
          position: absolute;
          top: 0;
          right: 0;
          background: #b4282d;
          text-align: center;
          font-size: 18rpx;
          color: #fff;
          line-height: 28rpx;
          border-radius: 50%;
        }

        img {
          display: block;
          height: 44rpx;
          width: 44rpx;
          position: absolute;
          top: 10rpx;
          left: 0;
        }
      }
     .icon{
       width:100%;
       height:50%;
        .img {
          display: block;
          height: 30px;
          width: 30px;
          position: relative;
          top: 5rpx;
          left: 0;
        }
      .img.shoucang{
       
          display: block;
          height: 30px;
          width: 30px;
          position: relative;
          top: 5rpx;
          left: 0;
          background: url('../../static/shoucang.png') no-repeat;
      }
      .dianzan{
        display: block;
          height: 30px;
          width: 30px;
          position: relative;
          top: 5rpx;
          left: 0;
          background: url('../../static/dianzan.png') no-repeat;
       
     }
     .dianzan.active{
        display: block;
          height: 30px;
          width: 30px;
          position: relative;
          top: 5rpx;
          left: 0;
          background: url('../../static/dianzan_fill.png') no-repeat;
       
     }
     p{
      text-align:center;
      margin:5px auto;
      font-family:"微软雅黑";
          font-size:12px;

      }
      }
    }
    .bar_fabiaopinglun{
      background-color:#515151;
      width:165px;
      height:60px;
       p{
      text-align:center;
      font-family:"微软雅黑";
          font-size:12px;
          color:#ffffff;
          line-height:50px;
      }
    }


}
</style>
