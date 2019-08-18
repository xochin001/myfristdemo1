<template>
	<view>
		<section class="sec-nav">
			<navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
		</section>
		<view class="content">
			<view class="userinfo">
					<img class ="user-img" @click="toLogin" :src="avator" alt="头像">
						<view class="userinfo-name">
							<p>{{userInfo.nickName||'点我登陆'}}</p>
						</view>
			</view>
				<view class="dingzhilist">
					<view class="liebiao" v-for="(item , index) in liebiaos" :key="index" >
						<img  class="liebiao-img" :src="item.img_url">
							<p class="p1">{{item.title}}</p>
								<view>
									<view class="price">
										<p >{{item.price}}起</p>
									</view>
										<p class="p2">{{item.context}}</p>
								</view>
						</view>
				</view>
				<view class="dingzhiform">
					<view>
						<view class="dingzhi-title">
							<h1>开发洽谈</h1>
						</view>
							<view class="phone">
								<p>手机:</p>
									<input type="number" v-model="telnumber" placeholder="请输入手机号">
							</view>
						<view class="uname">
								<p>称呼:</p>
									<input type="text" v-model="username" :placeholder="[userInfo.nickName?userInfo.nickName:'请输入称呼']">
						</view>
									<textarea class="text" max='150' name="" v-model="context" placeholder="请填写你的意见和反馈"></textarea>
						<view @click="submit" class="bottom">
						点击提交
						</view>
					</view>
				</view>
			<view class="kaifagaozhi">
				<view class="title">
					<p>开发准备</p>
				</view>
				<view>
					<p class="p11"> 1、首先需要准备个体户或者企业营业执照用于开通微信企业号</p>
					<p class="p11"> 2、需要理清小程序具体的面向内容，以便开发审核需要填写类目</p>
					<p class="p11"> 3、需要准备300元人民币用于向微信提交资质审核（这笔钱直接打到微信运营账号）</p>
					<p class="p11"> 4、需要协商准备开发内容后台数据的存储方式和架构，以便后期维护和管理</p>
					<p class="p11"> 5、确定开发后，所需要准备的各种内容资料，包括文字图片等等。以及协商UI风格</p>
				</view>	
		  </view>       
		<view class="kaifagaozhi2">
			<view class="title">
					<p >交易须知</p>
             </view>
				<view >	
					<p class="p11"> 1、项目开发过程中不允许更改功能模块，否则视为二次开发</p>
					<p class="p11"> 2、不接受先开发后付款的交易方式（可以协商预付）</p>
					<p class="p11"> 3、因沟通中可能出现不同的分歧。但请相信沟通后会提供让您满意的结果。</p>
					<p class="p11"> 4、我方没有违背卖家原有开发意愿，中途不允许退款。</p>
				</view>	
			</view>
		</view>
		<view class="kaifagaozhi3">
			<view class="title">
					<p >售后服务</p>
		     </view>
				<view>
					<p class="p11"> 1、保证系统正常运行，平台出现bug，操作等问题我方提供解决方案</p>
					<p class="p11"> 2、项目交付完成，同时交付后台链接账户密码。并免费提供后台操作培训</p>
					<p class="p11"> 3、平台后期界面或者功能微调，我方可帮客户修改，若大功能修改则确定功能需求，新签协议</p>
				</view>
			</view>
		</view>
</view>
</template>
<script>
	import navigationBar from '../../components/navigationBar.vue'
	import { toLogin, login, post ,get} from "../../api/homepage.js"
export default {
		onShow() {
    if (login()) {
      this.userInfo = login()
      this.avator = this.userInfo.avatarUrl
    }
    //console.log(this.userInfo)
  },
	  created() {},
	  mounted() {},
  components: {
    navigationBar,
  },
  data() {
    return {
      avator: "../../static/denglu.png",
      videoTitle: '我要定制',
      userInfo: {},
      username: '',
      telnumber: '',
      openid: '',
      context: '',
      liebiaos:{}
    }
  },
  methods: {
    toLogin() {
      if (!this.userInfo.avatarUrl) {
        wx.navigateTo({
          url: "/pages/login/login"
        });
      }
    },
    async submit() {
      var _this = this
          if(!this.userInfo.openId){
               wx.showToast({
              title: "请先登陆",
              icon:"none",
              duration: 1500,
              mask: true,
            })
               return false
          }else if(this.context===""){
           wx.showToast({
          title: "请填写你的意见和反馈",
          icon:"none",
          duration: 1500,
          mask: true,
        })
           return false
      }else if(this.telnumber===""){
           wx.showToast({
          title: "请填写你电话号码",
          icon:"none",
          duration: 1500,
          mask: true,
        })
           
         }else{
      const data = await post('/api/chen/dingzhi', {
        username: this.username,
        telnumber: this.telnumber,
        openid: this.userInfo.openId,
        nickname: this.userInfo.nickName,
        context: this.context
      })
      if (data) {
        wx.showToast({
          title: "提交成功",
          icon: "success",
          duration: 1500,
          mask: true,
          success: res => {
            _this.context = '',
              _this.telnumber = '',
              _this.username = ''
          }
        })
      }
    }
   },
   async get_liebiao(){
      const res = await get('/api/chen/splb',{
        owner : 'chenxing'
      })
      this.liebiaos = res.liebiao
   }
  },
  onLoad(){
    this.get_liebiao()
  }
};
</script>
<style lang="scss">
	.content {

	}
	
	page {
		background: #e7ebed;
	}
	
	.userinfo {
		background: #ffffff;
		border-radius: 10upx;
		height: 300upx;
		width:95%;
		margin: 10upx auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	
	.user-img {
		height: 120upx;
		width:120upx;
		border-radius: 50upx;
	}
	
	.userinfo-name {
		margin: 30upx auto;
		font-size: $uni-font-size-sm;
	}
	
	.dingzhilist {
		background: #ffffff;
		border-radius: 10upx;
		width:95%;
		margin: 10upx auto;
		height: 600upx;
		display: flex;
		flex-wrap: wrap;
	}
	
	.liebiao {
		margin: 10upx auto;
		width:32.5% ;
		border-bottom: 1px solid #e7ebed;
		float: left;
	}
	
	.liebiao-img {
		width: 100upx ;
		height: 100upx ;
		margin-left: 60upx;
	}
	
	.p1 {
		font-size: $uni-font-size-ssm;
		text-align: center;
	}
	
	.price {
		height: 40upx;
		text-align: center;
		font-size: $uni-font-size-sm;
		color: red;
	}
	
	.p2 {
		font-size :$uni-font-size-ssm;
	}
	
	.dingzhiform {
		@extend .dingzhilist ;
	}
	
	.dingzhi-title {
		font-size: $uni-font-size-lg;
		text-align: center;
		margin-top: 30upx;
		margin-bottom: 20upx;
	}
	
	.phone {
		display: flex;
		width:675upx;
		padding : 10upx 20upx ;
		font-size: $uni-font-size-sm;
		align-items: center;
		background: #f0f0f0;
	}
	
	input {
		margin-left: 40upx;
		height : 30upx ;
		width:500upx;
	}
	
	.uname {
		@extend .phone ;
		background: #ffffff;
	}
	
	input {
		
	}
	
	.text {
		background:  #f0f0f0;
		height: 250upx;
		border-bottom: 1px solid #e5e5e5;
		font-size: $uni-font-size-sm;
		width:715upx;
	}
	
	.bottom {
		margin: 20upx auto ;
		border-radius:10upx;
		background: #39B54A;
		text-align: center;
		padding-top:10upx;
		height: 80upx;
		color: #ffffff;
		font-size: $uni-font-size-sm;
	}
	
	.kaifagaozhi {
		@extend .dingzhilist ;
		
	}
	
	.title {
		margin : 40upx auto ;
		width : 200upx ;
		height:  60upx ;
		background: #3469FA;
		border-radius:30px 0;
		text-align: center;
		line-height: 40upx;
		font-size: $uni-font-size-base;
	}
	
	.title-con {
		
	}
	
	.p11 {
		margin-left : 40upx ;
		font-size: $uni-font-size-sm;
		color : #3469FA;
		text-align: left;
		margin-top: 40upx ;
		line-height: 25upx ;
	}
	
	.kaifagaozhi2 {
		@extend .dingzhilist ;
		
	}
	
	.kaifagaozhi3 {
		@extend .dingzhilist ;
		
	}
</style>
