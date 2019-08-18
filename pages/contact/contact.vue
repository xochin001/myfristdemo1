<template>
  <div>
    <section class="sec-nav">
      <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
    </section>
    <div class="content">
      <div class="userinfo">
        <div class="users">
          <img @click="toLogin" :src="avator" alt="头像">
          <div class="userinfo_name">
            <p>{{userInfo.nickName||'点我登陆'}}</p>
          </div>
        </div>
      </div>
      <div class="dingzhilist">
        <div class="liebiao" v-for="(item , index) in liebiaos" :key="index" >
          <img :src="item.img_url">
          <p class="p1">{{item.title}}</p>
          <div>
           <div class="price">
            <p class="p2">
              {{item.price}}
            </p>
            <p class="p21">起</p>
            </div>
              <p class="p3">
                {{item.context}}
            </p>
        </div>
      </div>
      </div>
      <div class="dingzhiform">
        <div>
          <div class="dingzhi_title">
            <h1>开发洽谈</h1>
          </div>
          <div class="phone">
            <p>手机</p>
            <input type="number" v-model="telnumber" placeholder="请输入手机号">
          </div>
          <div class="uname">
            <p>称呼</p>
            <input type="text" v-model="username" :placeholder="[userInfo.nickName?userInfo.nickName:'请输入称呼']">
          </div>
          <textarea class="text" max='150' name="" v-model="context" placeholder="请填写你的意见和反馈"></textarea>
          <div @click="submit" class="bottom">
            点击提交
          </div>
        </div>
      </div>
        <div class="kaifagaozhi">
          <div class="title">
              <p >交易须知</p>
              
              </div>
                <p class="p1"> 1、项目开发过程中不允许更改功能模块，否则视为二次开发</p>
                <p class="p1"> 2、不接受先开发后付款的交易方式（可以协商预付）</p>
                <p class="p1"> 3、因沟通中可能出现不同的分歧。但请相信沟通后会提供让您满意的结果。</p>
                <p class="p1"> 4、我方没有违背卖家原有开发意愿，中途不允许退款。</p>
          </div>
             
      <div class="kaifagaozhi2">
          <div class="title">
              <p >售后服务</p>
              
              </div>
                <p class="p1"> 1、项目开发过程中不允许更改功能模块，否则视为二次开发</p>
                <p class="p1"> 2、不接受先开发后付款的交易方式（可以协商预付）</p>
                <p class="p1"> 3、因沟通中可能出现不同的分歧。但请相信沟通后会提供让您满意的结果。</p>
                <p class="p1"> 4、我方没有违背卖家原有开发意愿，中途不允许退款。</p>
          </div>
          

    </div>
</div>
 
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
  mounted() {

  },
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
        background-color: #e7ebed;
        width: 100vw;
        height: 400vh;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;

  .userinfo {
        background-color: #FFFFFF;
        border-radius: 8px;
        display: flex;
        height: 130px;
        width: 95%;
        margin: 0 auto;

    .users {
        position: flex;
        height: 200rpx;
        width: 70vw;
        margin: 20rpx auto;

      //border: 1px solid blue;
    img {
        position: relative;
        height: 148rpx;
        width: 148rpx;
        border-radius: 50px;
        left: 180rpx;
        top: 10rpx;
        overflow: hidden;
      }

      .userinfo_name {
        position: relative;
        height: 60px;
        width: 60px;
        left: 185rpx;
        line-height: 20px;
        font-size: 14px;
        text-align: center;
      }
    }
  }

  .dingzhilist {
        display: flex;
        background-color: #FFFFFF;
        width: 95%;
        height: 460;
        border-radius: 8px;
        margin: 20px auto;
        flex-wrap: wrap;
        text-align: center;

    .liebiao {
        width: 32.5%;
        //height:33.33%;
        position: relative;
        padding: 20rpx 0 30rpx;
        flex-direction: column;
        display: flex;
        transition: all .6s ease-in-out 0s;
        transform: translateX(0rpx);
        border-bottom: 1px solid #e7ebed;
        align-items: center;
        font-size:11px;
        font-family:"微软雅黑";
        font-weight:400;
      img{
        width:70px;
        height:80px;
      }
      .p1{
        width:110px;
        height:20px;
      }
      div{
        width:110px;
        height:60px;
        margin-top:-2px;  
      }
      .price{
        display:flex;
        width:110px;
        height:20px;
        align-items: center;
        display:inline-box;
       .p2{
          margin-left:20px;
          width:33.33%;
          color:red;
       }
       .p21{
        width:10px;
       }
      }
      .p3{
        width:110px;
        height:30px;
      }
    }
  }
  .dingzhiform {
        display: flex;
        background-color: #FFFFFF;
        width: 95%;
        height: 330px;
        border-radius: 8px;
        margin: 0 auto;
        font-family: "微软雅黑";
    .dingzhi_title {
        position: flex;
        text-align: center;
        line-height: 2.58823529em;
    }
    .phone {
      position: relative;
      padding: 10px 15px;
      font-size: 14px;
      width: 328px;
      display: flex;
      align-items: center;
      margin-top: 10px;
      padding-top: 0px;
      padding-bottom: 0;
      background: #F0F0F0;
      border-top: 1px solid #e5e5e5;
      border-bottom: 1px solid #e5e5e5;

      input {
        height: 3.58834529em;
        min-height: 3.58823529em;
        line-height: 3.58823529em;
        margin-left: 30px;
        width: 140px;
        // border-left :1px solid #e5e5e5;
      }
    }

    .uname {
      position: relative;
      padding: 10px 15px;
      font-size: 14px;
      width: 328px;
      display: flex;
      align-items: center;
      padding-top: 0px;
      padding-bottom: 0;

      border-bottom: 1px solid #e5e5e5;

      input {
        height: 3.58834529em;
        min-height: 3.58823529em;
        line-height: 3.58823529em;
        margin-left: 30px;
        width: 140px;
        // border-left :1px solid #e5e5e5;
      }
    }

    .text {
      position: relative;
      padding-top: 0px;
      padding-bottom: 0;
      padding: 10px 15px;
      height: 200rpx;
      width: 328px;
      border-bottom: 1px solid #e5e5e5;
      background: #F0F0F0;
    }

    .bottom {
      position: relative;
      padding-top: 0px;
      padding-bottom: 0;
      padding: 10px 15px;
       text-align: center;
       width:60px;
       margin:9px auto ;
      height:20px;
      //border: 1px solid red;
      border-radius:8px;
      background:#39B54A;
      color:#ffffff;
      font-size:14px;
    }
  }
    .kaifagaozhi{
      //position:flex;
      position: relative;
      margin:20px auto ;
      width:95%;
      height:300px;
      border-radius: 8px;
      background: #ffffff;
    .title {
      position:relative;
      margin:20px auto;
      width:140px;
      height:40px;
      background: #39B54A;
      border-radius:30px 0;
      text-align: center;
      line-height:40px;
      font-size:14px;
      color:#ffffff;
     }
    .p1{
      position:relative;
      left:20px;
      font-size:12px;
      color:#2719D0;
      width:300px;
      }
    }

  .kaifagaozhi2{
      //position:flex;
      margin:20px auto ;
      width:95%;
      height:300px;
      border-radius: 8px;
      background: #ffffff;
    .title {
      position:relative;
      margin:20px auto;
      width:140px;
      height:40px;
      background: #39B54A;
      border-radius:30px 0;
      text-align: center;
      line-height:40px;
      font-size:14px;
      color:#ffffff;
     }
    .p1{
      position:relative;
      left:20px;
      font-size:12px;
      color:#2719D0;
      width:300px;
      }
    }


}

</style>
