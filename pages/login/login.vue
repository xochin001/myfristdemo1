<template>
  <div>
  <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
  <div class="login">
      
    <div class="logo">
    </div>
    <button class="login-btn" open-type="getUserInfo" lang="zh_CN" @getuserinfo="doLogin">微信登录</button>
    <button class="login-btn" @click="goTo()">返回登录</button>
    <button class="login-btn" @click="clear()">清除缓存</button>
  </div>
  </div>
</template>

<script>
 import navigationBar from '../../components/navigationBar.vue'
 import {login,host} from "../../api/homepage.js"
 var qcloud = require("wafer2-client-sdk/index.js")
  export default {
    created() {},
    mounted() {
      qcloud.setLoginUrl(host + "/api/login")
    },
    data() {
      return {
        videoTitle: '请登陆',
        
      }
    },
    components: {
      navigationBar,
    },
    methods: {
      doLogin() {
        wx.showLoading({
          title: "登录中...", //提示的内容,
          mask: true, //显示透明蒙层，防止触摸穿透,
          success: res => {}
        });
        const session = qcloud.Session.get();
        if (session) {
          // 第二次登录
          // 或者本地已经有登录态
          // 可使用本函数更新登录态
          qcloud.loginWithCode({
            success: res => {
              // this.setData({ userInfo: res, logged: true });
              wx.setStorageSync("key", "value")
            },
            fail: err => {
              console.error(err)
            }
          });
        } else {
          // 首次登录
          qcloud.login({
            success: res => {
              console.log(res)

              wx.hideLoading()
              wx.setStorageSync("userInfo", res)
              var openId = res.openId
              wx.setStorageSync("openId", openId)
              wx.navigateBack({})
            },
            fail: err => {
              console.log("出错了"+err)
              wx.hideLoading()
              wx.navigateBack({})
            }
          })
        }
      },
      goTo(){
        wx.navigateBack({})
      },
      clear(){
        wx.clearStorage()
      }
    },
    computed: {}
  };

</script>
<style lang='scss'>
  .login {
    background:url('../../static/background.jpg');
    background-repeat:repeat;
  width: 100wv;
  height: 100vh;
  box-sizing: border-box;
  padding-top: 1rpx;
  .logo {
    width: 230rpx;
    height: 80rpx;
    background: url('../../static/denglu.png') no-repeat;
    background-size: 100% 100%;
    margin: 200rpx auto 0 auto;
  }
  .login-btn {
    text-align: center;
    background: #b4a078;
    width: 30%;
    height: 80rpx;
    line-height: 80rpx;
    color: #fff;
    font-size: 28rpx;
    margin-top: 30rpx;
  }
}

</style>
