<template>
	<view>
		<image class="background" src="@/static/jpg/login/login_2.jpg" mode="widthFix">
		</image>
		
		<view class="login">
		    <view class="user">
		        <!-- <span class="login_iconfont psl_font icon-fukuan"></span> -->
		        <u-input 
					class="user_input"
		            placeholder-class="text" 
		            type="number" 
					maxlength="5"
		            :placeholder="m_login.user"
		            v-model ="Tid"/>
		    </view>
		
		    <view class="psw">
		        <!-- <span class="login_iconfont psl_font icon-shuohuaqipao"></span> -->
		        <u-input 
					:clearable="false"
					maxlength="6"
					class="psw_input"
		            placeholder-class="password" 
		            type="password" 
		            :placeholder="m_login.psw"
		            v-model = "Tpwd"/>
		        
				<u-tag class="captcha" :text="m_login.captcha" 
					type="primary" @tap="getCaptcha"/>
				
		    </view>
		
		    <view class="hint">
		        {{hint}}
		    </view>
		
		
		    <view class="loginBtn" url="../../pages/index/index" @tap ="login" hover-class="navigator-hover" open-type="navigate">
		        登录
		    </view>
		
		    <view class="others">
		
		        <navigator class="others_email" url="../../pages/emailChangeLogin/emailChange" hover-class="navigator-hover" open-type="navigate">
		            想换邮箱？
		        </navigator>
		        
		        
		        <navigator class="others_psw" url="../../pages/pwdChangeLogin/pwdChange" hover-class="navigator-hover" open-type="navigate">
		            密码忘了？
		        </navigator>
		    </view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				app: {},
				Tid:"",
				Tpwd:"",
				hint:"",
				m_login:{
					user:"请输入职工号",
					psw:"请输入邮箱验证码",
					captcha:"获取验证码",
				},

				user_psw: [
				  {
					"src": "icon-fukuan",
					"text": "用户",
				  },
				  {
					"src": "icon-shuohuaqipao",
					"text": "密码",
				  },
				]
			};
		},
		methods:{
			setHint:function (hint_) {
			  this.hint=hint_; 
			},
			
			login: function(){
			  var this_ = this;
			  
			  if(this.Tid==""){
				this_.setHint("您的职工号似乎没输入:)")
			  }
			  else if(this.Tpwd==""){
				this_.setHint("您的验证码似乎没输入:)")
			  }
			  else {
				uni.request({
				  method:'post',
				  url: this_.app.url+"/loginBypwd", 
				  data: {
					"Tid": this_.Tid,
					"Tpwd": this_.Tpwd
				  },
				  header: {
					  'content-type': 'application/x-www-form-urlencoded'
				  },
				  success: function(res) {
					let info = res.data;  
					if(info.code==0){
					  try {
						// console.log(info)
						uni.setStorageSync('a', info.info.a)
						uni.setStorageSync('r', info.info.r)
						uni.setStorageSync('i', this_.Tid)
						
						this_.setHint("欢迎老师 如果页面长时间没跳转 麻烦老师通知技术人员处理");
						this_.app.Tid=this_.Tid;
						this_.app.Tname=info.Tname;
						this_.Tpwd = " ";
						
						uni.switchTab({
						  url: '/pages/index/index'
						});
					  } catch (e) { console.log(e);}
					  
					}
					else this_.setHint(info.info)
				  }
				})
			  }
			},
			getCaptcha: function(){
			  var this_ = this;
			  if(this.Tid){
				uni.request({
				  method:'post',
				  url: this_.app.url+"/getCaptcha", //仅为示例，并非真实的接口地址
				  data: {
					"Tid": this_.Tid
				  },
				  header: {
					  'content-type': 'application/x-www-form-urlencoded'
				  },
				  success: function(res) {
					if(res.data.code==0){
					  this_.setHint(res.data.info);  
					}
				  }
				})
			  }
			  else this.setHint("您的职工号似乎没输入:)");
			}
		},
		
		onLoad: function (options) {
		  var this_ = this;
		  this.app = getApp().globalData;
		  
		  var i = uni.getStorageSync('i');
		  var a = uni.getStorageSync('a');
		  var r = uni.getStorageSync('r');
		  if(r!=""&&i!=""){
			// console.log("2");
			uni.request({
			  method:'post',
			  url: this_.app.url+"/loginByaccess", //仅为示例，并非真实的接口地址
			  data: {
				"Tid": i,
				"access": a
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
				  // console.log("3");
				  try{
					uni.setStorageSync('a', res.data.info.a)
					uni.setStorageSync('r', res.data.info.r)
					this_.app.Tid=i;
					this_.app.Tname=res.data.info.Tname;
					
					uni.switchTab({
					  url: '/pages/index/index'
					});
				  }
				  catch(e){console.log(e);}
				}
				else{
				  // console.log("4");
				  uni.request({
					method:'post',
					url: this_.app.url+"/refresh", 
					data: {
					  "Tid": i,
					  "refresh": r
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: function(res) {
					  if(res.data.code==0){
						uni.setStorageSync('a', res.data.info.a)
						uni.setStorageSync('r', res.data.info.r)
						
						this_.app.Tname=res.data.info.Tname;
						uni.switchTab({
						  url: '/pages/index/index'
						});
					  }
					  else{
						// console.log("5");
					  }
					}
				  })
				}
			  }
			})
		  }
		},
	}
</script>

<style lang="less">
	@import url("@/common/uni.less");
	@import "./login.less";
</style>
