  <template>
  <div>
     <ul>
        <scroll-view class="scroll-view" :scroll-y="true" >
          <li v-for="(item,index) in descimgs" :key="index">
             <img class="descimgs" :src="item.img" lazy-load:true>
          </li>
        </scroll-view>
      </ul>
       <div class="liucheng">
          <div class="title">
            <p>服务流程</p>
          </div>
          <div class="subtitle">
            <p>CASE&nbspPRESENTATION</p>
          </div>
          <div class="liucheng_content">
             <div class="liucheng_img" v-for="(item , index) in cases" :key="index">
               <p> {{item.title}}</p>
             </div>
             <div style="clear:both"></div>
             <div class="liucheng_shuoming" v-for="(item , index1) in cases" :key="index1">
              <div class="liucheng_shuoming_title">
                <p>
                  {{item.title}}
                </p>
              </div>
              <div class="liucheng_shuoming_wenzi">
                <p>
                  {{item.context}}
                </p>
              </div>
            </div>
            <div style="clear:both"></div>
            <div class="yingyongchangjing">
              
            </div>
          </div>
        </div>
      </div>

  </template>
  <script>
    export default {
		 data(){
		        return{
		          descimgs:[
		                {img:"https://www.jingsoftware.com/chen_xcy/img/context/des1.png"},
		                {img:"https://www.jingsoftware.com/chen_xcy/img/context/des2.png"},
		                {img:"https://www.jingsoftware.com/chen_xcy/img/context/des3.png"}
		
		          ],
		           cases: [{
		        title:"项目沟通",
		        context:"说出您的需求，我们会立即和您进行专业沟通，帮您挖掘需求和专业性的建议。"
		      },{
		        title:"需求分析",
		        context:"我们会针对您的需求给出完备的解决方案，您只需确认这份需求是否达到您的要求即可。"
		      },{
		        title:"项目执行",
		        context:"项目确认后我们会立即成立项目并展开会议，设计开发测试逐步安排进行。"
		      },{
		        title:"客户确认",
		        context:"界面完成后和客户沟通确认，在和客户沟通过程中继续优化设计。"
		      },{
		        title:"内部测试",
		        context:"客户确认后，开发组开始进行开发并对测试反馈问题一一解决。"
		      },{
		        title:"交付完成",
		        context:"测试通过后，部署到服务器当中待客户审核确认后，交付完成。"
		      },
		
		      ]
		        }
		      }
     }
  </script>
  <style lang="scss">
  
  </style>
