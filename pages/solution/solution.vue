<template>
  <view>
    <section class="sec-nav">
      <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
    </section>
    <view class= "fangan-titles">
            <ul  @click = 'to_fangan_detail(item.fangan_id)' class="fangan" v-for="(item , index) in fangans" :key="index">
                <li  class="title" >
                    <view class =" fangan-content">{{item.fangan_title}}</view>
                    <view class ="sub-title">{{item.sub_title}}</view>
               </li>
                <img class="fangan-img" :src="item.img_url">
            </ul>
       </view>
   
    </view>
  </view>
</template>
<script>
import navigationBar from '../../components/navigationBar.vue'
import { get } from '../../api/homepage.js'

export default {

  components: {
    navigationBar,
  },
  data() {
    return {
      videoTitle: '解决方案',
      fangans:{},


    }
  },
  methods: {
    async get_fangan_detail(){
                  const res = await get ('/api/chen/fangan_detail',{
                  })
              this.fangans = res.data
            },
            to_fangan_detail(id){
          // console.log("跳转方案细节"+ id)
          wx.navigateTo({
            url:"../fangan/main?id=" +id
          })
            }
    
  },
  onLoad(){
    this.get_fangan_detail()
  }

};

</script>
<style lang="scss">
	.fangan-titles{
		flex-direction: column;
	 }
	page {
		background: #e7ebed;
	}
	
	.fangan {
		background: #ffffff;
		height: 150upx;
		display: flex;
		margin: 20upx auto;
		flex-direction: row;
		border-radius: 10upx;
		
	}
	
	.title {
		width: 675upx;
		
	}
	
	.fangan-content {
		margin-left : 30upx;
		margin-top: 30upx;
		font-size: $uni-font-size-sm;
	}
    
	.sub-title {
		@extend .fangan-content ;
		font-size: $uni-font-size-ssm;
	}
	
	.fangan-img {
		float: right;
		width : 220upx;
		height: 150upx;
		padding: 0 20upx;
	}


</style>
