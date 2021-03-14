<template>
	<view>
		<image class="background" src="@/static/jpg/login/login_2.jpg" mode="widthFix">
		</image>
		
		<view class="login">
		    <view class="user">
		        <span class="login_iconfont psl_font icon-psw"></span>
		        <u-input 
					class="user_input"
		            placeholder-class="text" 
		            type="number" 
					maxlength="5"
		            :placeholder="m_login.user"
		            v-model ="Tid"/>
		    </view>
		
		    <view class="psw">
		        <span class="login_iconfont psl_font icon-psw"></span>
		        <u-input 
					:clearable="false"
					maxlength="6"
					class="psw_input"
		            placeholder-class="password" 
		            type="password" 
		            :placeholder="m_login.psw"
		            v-model = "Tpwd"/>
					
					<u-toast ref="uToast"></u-toast>
					<u-verification-code seconds="30" @end="end" @start="start" ref="uCode" 
					@change="codeChange"
					change-text="X秒后重新获取"
					start-text="点击获取验证码"
					end-text="再次获取验证码"></u-verification-code>
					
					<view class="captcha">
						<u-button 
							ripple="true"
							plain="true"
							size="mini"
							:loading="getCaptchaBtnDisabled"
							:disabled="getCaptchaBtnDisabled"
							:type="getCaptchaBtnStatus" 
							@click="getCaptcha">
							{{getCaptchaHint}}
						</u-button>
					</view>
					
				
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
				getCaptchaStyle: {
					flex: "3",
					textAlign: "center",
					height: "35px",
					lineHeight: "35px",
					fontSize: "12px"
				},
				getCaptchaBtnDisabled: false,
				getCaptchaBtnStatus: "primary",
				getCaptchaHint: "获取验证码",
				app: {},
				Tid:"",
				Tpwd:"",
				hint:"",
				m_login:{
					user:"请输入职工号",
					psw:"请输入邮箱验证码"
				},
			};
		},
		methods:{
			codeChange(text) {
				console.log("codeChange")
				this.getCaptchaHint = text;
			},
			end() {
				this.getCaptchaBtnStatus="primary";
				this.getCaptchaBtnDisabled=false;
			},
			start() {
				this.getCaptchaBtnStatus="info";
				this.getCaptchaBtnDisabled=true;
			},
			
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
				  url: this_.app.url+"/loginByPwd", 
				  data: {
					"Tid": this_.Tid,
					"Tpwd": this_.Tpwd
				  },
				  header: {
					  'content-type': 'application/x-www-form-urlencoded'
				  },
				  success: function(res) {
					if(res.data.code==0){
						uni.setStorageSync('i', this_.Tid);
						this_.Tpwd = " ";
						this_.toIndex(res);
						this_.setHint("欢迎老师 如果页面长时间没跳转 麻烦老师通知技术人员处理");
						
						
					}
					else this_.setHint(info.info)
				  }
				})
			  }
			},
			getCaptcha: function(){
			  var this_ = this;
			  if(this.Tid){
					uni.showLoading({
						title: '正在获取验证码'
					})
					
				  
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
						setTimeout(() => {
							uni.hideLoading();
							// 这里此提示会被this.start()方法中的提示覆盖
							this_.$u.toast('验证码已发送');
							this_.setHint(res.data.info);  
							// 通知验证码组件内部开始倒计时
							this_.$refs.uCode.start();
						}, 1000);
						
					}
				  },
				  fail:function(){
					this.$u.toast('验证码发送失败 网络因素？');
				  }
				})
			  }
			  else this.setHint("您的职工号似乎没输入:)");
			},
			
			toIndex:function(res){
				try{
					uni.setStorageSync('a', res.data.info.a)
					uni.setStorageSync('r', res.data.info.r)
					this.app.Tid=uni.getStorageSync('i');
					this.app.Tname=res.data.info.Tname;
					console.log(this.app.Tid);
					console.log(uni.getStorageSync('i'));
					
					console.log(this.app.Tname);
					uni.switchTab({
					  url: '/pages/index/index'
					});
				}
				catch(e){console.log(e);}
			}
		},
		
		onLoad: function (options) {
		  let this_ = this;
		  this.app = getApp().globalData;
		  
		  let i = uni.getStorageSync('i');
		  let a = uni.getStorageSync('a');
		  let r = uni.getStorageSync('r');
		  if(r!=""&&i!=""){
			// console.log("2");
			uni.request({
			  method:'post',
			  url: this_.app.url+"/loginByAccess", //仅为示例，并非真实的接口地址
			  data: {
				"Tid": i,
				"access": a
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
				  this_.toIndex(res);
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
							this_.toIndex(res);
						}
						else{
							console.log("refresh token failed");
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
