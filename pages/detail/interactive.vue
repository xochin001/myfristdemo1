  <template>
    <view>
		<view class="article-title">	
			<ul class="article" @click="toArticleDetail(item.article_id)" v-for="(item , index) in articles" :key="index">
				<li class="title">
					<view class ="title-content">{{item.title}}</view>
					<p class="create-time">{{times[index]}}</p>
					<p class="liulan">浏览量:{{item.click}}</p>
				</li>
				<img :src="item.suoluetu" alt="">
			</ul>
		</view>
      </view>
  </template>
  <script>
	  import {get} from '../../api/homepage.js'
	  import {formatTime} from '../../common/utils/index.js'
 
    export default {
      data() {
		  return {
			  articles : {},
			  times:[]
		  }
	  },
	  created(){
		  this.getArticle()
	  },
	  methods:{
		  async getArticle(){
			  let _this = this
			  const res = await get('/api/chen/article')
			 
			  _this.articles = res.data.reverse()
			  _this.articles.forEach(value=>{
				  let time = formatTime(new Date(value.create_time))
				  _this.times.push(time)
			  })
		  },
		  toArticleDetail(id){
			  console.log("试试看跳转情况了"+id)
			  wx.navigateTo({
			  	 url : "../article/article?id=" +id
			  })
		  }
	  }

  };
  </script>
  <style lang="scss">
	  .aritcle-title {
		  
	  }
	  
	  .article {
		  height: 180upx;
		  background: #ffffff;
		  margin: 10upx auto;
		  border-radius: 4upx;
		  display: flex;
	  }
	  
	  .title {
		  padding-left : 10upx;
		  padding-right: 10upx;
		  width: 450upx;
	  }
	 
	 .title-content {
		 height: 50upx;
		 width: 400upx;
		 margin-top: 30upx;
		 font-size: $uni-font-size-sm;
	 }
	 
	 .create-time {
		 width:150upx;
		 margin-top: 70upx;
		 font-size: $uni-font-size-ssm;
		 float: left;
	 }
	 
	 .liulan {
		 width:150upx;
		 margin-top: 70upx;
		 font-size: $uni-font-size-ssm;
		 float: right;
	 }
	 
	 img {
		 width : 190upx;
		 height: 180upx;
		 padding: 0 20upx;
	 }
			 

  </style>

























<!--   <template>
    <div class= "content">
     <div @click="tobrandList" class="brand">
        品牌制造商直供
      </div>
    </div>
  </template>
  <script>
    import {get} from '../../api/homepage'
    export default {
       data(){
        return{
          articles : {}
        }
       },

      methods: {

         tobrandList() {
      wx.navigateTo({
        url: "/pages/article/main"
      });
      }
}

 };
  </script>
  <style lang="scss">
    .content{
     // width : 100vw ;
      background-color: #e7ebed;
      display: flex;
      justify-content:space-around;
      border : 1px solid blcak ;

    }
.brand {
    width: 100%;
    margin-top: 20rpx;
    background: #ffffff;
  }
  </style>
 -->