<template>
  <div>
    <section class="sec-nav">
      <navigationBar :title="videoTitle" :titleColor="'black'"></navigationBar>
    </section>
    <div class= "fangan_titles">
            <ul  @click = 'to_fangan_detail(item.fangan_id)' class="fangan" v-for="(item , index) in fangans" :key="index">
                <li  class="title" >
                    <div class ="web-font2 fangan_context">{{item.fangan_title}}</div>
                    <div class ="web-font sub_title">{{item.sub_title}}</div>
               </li>
                <img class="fangan_img" :src="item.img_url">
            </ul>
       </div>
   
    </div>
  </div>
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
  .fangan_titles{
      width:100%;
      margin: 0 auto ;
      background-color: #e7ebed;
    .fangan{
      width:100%;
      height:100px;
      background:#ffffff;
      margin: 10px auto ;
      display:flex;
       flex-direction:row;
       border-radius: 2px;
        .title {
          width: 100%;
          height:100px;
          display:flex;
           flex-direction:column;
          font-size:14px;
           .fangan_context{
              width:100%;
              height:60px;
              margin:0 auto ;
              text-align:center;
              color: #515151;
             font-family:"微软雅黑";
              padding-top:10px;
              border-bottom:1px solid #e7ebed;
            }
            .sub_title{
              width:100%;
              height:60px;
              margin:0 auto ;
              padding-top:10px;
              font-family:"微软雅黑";
            text-align:center;
            line-height:20px;
            color: #515151;
            }
          }
      .fangan_img{
           width:160px;
           height:100px;
           padding:0 10px;
        }
      }
   }


</style>
