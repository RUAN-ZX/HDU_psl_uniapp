<template>
	<view>
		<view class="header">
		    <image class="imageHeader" src="@/static/jpg/me.jpg" mode="scaleToFill" lazy-load="true">
		        
		    </image>
		    <view class="user">
		        <image class="avatar" src="@/static/jpg/avatarDefault.jpg" mode="scaleToFill" lazy-load="true">
		
		        </image>
		        <view class="name">
		            {{name}}
		        </view>
		    </view>
		
		    <view class="info">
		        <view 
		            class="item"
		            v-for="(item,index) in list_info"
		            v-key="index"
		            
		        >
		            <view class="value ellipsis">{{item.name}}：{{item.value}}</view>
		            <!-- <view class="name"></view> -->
		        </view>
		
		    </view>
		    
		
		</view>
		
		<view class="card card_1">
			<view 
				v-for="(item,index) in list_card1" 
				class="div_wrapper_1"
				v-key="index"
				@click = "btnTap(index)"
				>
				<span :class="'div_1_iconfont psl_font '+item.src"></span>
				<view class="div_text_1">{{item.text}}</view>
				
			</view>
		
		</view>
		
		<view class="inscribe">
			</br>浙ICP备2021000090号-1
			</br>杭州电子科技大学科技园 3楼320室
		</view>
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				test: [],
				url:"",
				name:"",
				user: {},
				app: {},
				list_info:[
					{
						value:"",
						name:"工号"
					},
					{
						value:"",
						name:"邮箱"
					}
				],
				list_card1: [
					{ 
					  src: "icon-emailChange",
					  text: "设置邮箱",
					},
					{
					  src: "icon-psw",
					  text: "设置密码",
					},
					{
					  src: "icon-feedback",
					  text: "联系反馈",
					},
					{  
					  src: "icon-exit",
					  text: "退出登录",
					},
				  
				],   
			};
		},
		methods:{
			btnTap: function (index){
			    switch(index) {
			      case 0:{
			        uni.navigateTo({
			          url: '/pages/emailSetting/emailSetting',
			        })
			        break;
			      }   
			      case 1:{
			        uni.navigateTo({
			          url: '/pages/pwdSetting/pwdSetting',
			          })
			        break;
			      }
			          
			      case 2:{
			        uni.navigateTo({
			          url: '/pages/feedback/feedback',
			        })
			        break;
			      }
			      case 3:{
					uni.setStorageSync('i', "");
					uni.setStorageSync('a', "");
					uni.setStorageSync('r', "");
			        uni.navigateTo({
			          url: '/pages/login/login',
			        })
			        
			        break;
			      }
			          
			      default:{
			        break;
			      }
			        
			    }   
			}
		},
		// 注意 onLoad: function (options){ 和 onLoad: () => { 效果不同！！！
		onLoad: function (options){
			_this = this;
			
			let access = uni.getStorageSync('a').toString();
			_this.app = getApp().globalData;
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/getEmailById", //仅为示例，并非真实的接口地址
			  data: {
				"Tid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
					
					_this.name=_this.app.Tname;
					_this.list_info[0].value = _this.app.Tid;
					_this.list_info[1].value = res.data.info;
					
				}
				else getApp().serverError("getEmailById",res);
				
			  },
			  fail:function (res) {
				getApp().networkError("getEmailById",res);
			  }
			});
		}
	}
</script>
<style lang="less">
	@import "@/common/uni.less";
	@import "./me.less";
	
</style>


