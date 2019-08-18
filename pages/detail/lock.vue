  <template>
    <view>
		<view class="fangan-title">
			<ul class="fangan" @click='toFangAn(item.fangan_id)' v-for = "(item , index) in fangans" :key='index'>
				<li class="title">
					 <view class="fangan-content">{{item.fangan_title}}</view>
						<view class="sub-title">{{item.sub_title}}</view>
				</li>
					<img :src="item.img_url" alt="" class="fangan-img">
			</ul>
		</view>
    </view>
  </template>
  <script>
  import {get} from '../../api/homepage.js'
    export default {
		created(){
			this.getFanganDetail()
		},
		data(){
			return{
				fangans:{}
			}
		},
		methods : {
			async getFanganDetail(){
				const res = await get('/api/chen/fangan_detail')
			this.fangans = res.data
		},
		  toFangAn(id) {
			  uni.navigateTo({
			  	url: '../fangan/fangan?id=' +id,
			  });
		  }
		}

    };
  </script>
  <style lang="scss">
		.fangan-title {
			flex-direction: column;
		}
		
		.fangan {
			height: 150upx;
			display: flex;
			background: #ffffff;
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
