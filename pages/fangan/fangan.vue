<template>
  <div>
      <div class="content">
        <u-parse :content="context"/>
      </div>
  </div>
</template>
<script>
import {get } from '../../api/homepage'
import uParse from '@/components/u-parse/u-parse.vue'

export default {
    onShow() {
    this.id = this.$root.$mp.query.id
    this.get_fangan()
  },
   components: {
    uParse
  },
    data(){
      return{
        fangans:{},
        context:'',
        title:''

      }
    },
    methods: {

      async get_fangan (){
         const res = await get('/api/chen/fangan',{
            fangan_id : this.id
         })
         this.fangans = res.data 
         this.context = this.fangans[0].fangan_content
         this.title = this.fangans[0].fangan_title
        wx.setNavigationBarTitle({
           title: this.title
         })
      }

    }
 };

</script>
<style lang="scss">
@import url("../../components/u-parse/u-parse.css");
  .content {
      width: 100vw;
      box-sizing: border-box;
      justify-content: center;
      padding-top: 1rpx;
      background-color: #e7ebed;
      }
  
</style>
