<template>
	<view>
		<header-ryan title="密码设置 固定密码"
			:top="cap_info.top"
			:left="cap_info.height"
			:hasBack="true">
		</header-ryan>
		 
		<view class="content">
			
			<u-form :model="model" :rules="rules" ref="uForm" :errorType="errorType">
				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" :left-icon="left_icon" label-width="120" 
				:label-position="labelPosition" 
					label="您的教工号" prop="name">
					<u-input :border="border" 
					placeholder="请输入您的教工号" 
					v-model="Tid" type="text"></u-input>
				</u-form-item>
				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}"
					:left-icon="left_icon"
					label-width="120" 
					:label-position="labelPosition"
					label="您的旧邮箱验证" prop="name">
					
					<u-input
						:border="border"
						placeholder-class="password" 
						type="text" 
						placeholder="请输入您的旧邮箱"
						v-model = "Temail_old"/>
						
					<view class="psw_captcha">
						<u-input
							:border="border"
							maxlength="6"
							class="psw_input"
							placeholder-class="password" 
							type="text" 
							placeholder="请输入旧邮箱接收到的验证码"
							v-model = "Tcaptcha_old"/>
						
						<u-toast ref="uToast"></u-toast>
						<u-verification-code seconds="30" @end="end" @start="start" ref="uCode" 
							@change="codeChange"
							change-text="X秒后重新获取"
							start-text="点击获取验证码"
							end-text="再次获取验证码">
						</u-verification-code>
						
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
				</u-form-item>
				
				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" 
					:left-icon="left_icon"
					label-width="120" 
					:label-position="labelPosition"
					label="您的密码设置" prop="name">
					
					<u-input
						:border="border"
						placeholder-class="password" 
						type="password" 
						placeholder="请输入您的想设置的密码"
						v-model = "Temail_new"/>
						
					<view class="psw">
						<u-input
							:border="border"
							placeholder-class="password" 
							type="password" 
							placeholder="请再次输入您的想设置的密码"
							v-model = "Temail_new"/>
					</view>
					
						
					
				</u-form-item>
				
				
				
				<u-form-item 
					:left-icon="left_icon"
					label-width="120"
					:leftIconStyle="{color: '#888', fontSize: '32rpx'}" 
					:label-position="labelPosition" label="备注" prop="intro">
					<u-input type="textarea" :border="border" 
					placeholder="您还有什么要补充的嘛" 
					v-model="Tcomment" />
				</u-form-item>
				

				
			</u-form>
			
			<u-button @click="submit"
			type="primary"
			:plain="false"
			:ripple="true">提交</u-button>
			
		</view>
	</view>
</template>

<script>
var _this;
import headerRyan from "@/components/header/header.vue";

export default {
	components:{
		headerRyan
	},
	data() {
		return {
			left_icon:"tags",
			app: {},
			cap_info:{},
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
			
			Tid: "",
			Temail_old: "",
			Temail_new: "",
			
			Tcaptcha_old: "",
			Tcaptcha_new: "",
			Tcomment: "",
			
			border: true,
			check: false,
			selectStatus: 'close',
			
			actionSheetShow: false,
			pickerShow: false,
			selectShow: false,
			radioCheckWidth: 'auto',
			radioCheckWrap: false,
			labelPosition: 'top',
			codeTips: '',
			errorType: ['border'],
		};
	},
	onLoad: function () {
		_this = this;
		_this.app = getApp().globalData;
		_this.cap_info = _this.app.cap_info;
		
	},
	
	methods: {
		submit() {
			
		},
		// 点击actionSheet回调
		actionSheetCallback(index) {
			uni.hideKeyboard();
			this.model.sex = this.actionSheetList[index].text;
		},
		

		codeChange(text) {
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
		getCaptcha: function(){
			var this_ = this;
			if(this.Tid){
				uni.showLoading({
					title: '正在获取验证码'
				});
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
					
				}else{
					uni.hideLoading();
					this_.$u.toast(res.data.info);
					this_.setHint(res.data.info); 
				}
			  },
			  fail:function(){
				this.$u.toast('验证码发送失败 网络因素？');
			  }
			})
		  }
		  else this.setHint("您的职工号似乎没输入:)");
		},
	}
};
</script>
<style lang="less">
	@import url("@/common/uni.less");
	.content {
		width: 100vw;
		padding: 0 @padding @padding @padding;
	}
	.psw{
		margin-top: @padding;
	}
	.psw_captcha{
		margin-top: @padding;
		display: flex;
		flex-direction: row;
		.psw_input{
			flex: 5;
			font-size: 16px;
	    }
	    .password{
	      color: @labelColor;
	      font-size: 16px;
	    }
		
	    .captcha{
		  flex: 3;	
		  display: flex;
		  justify-content: center;
		  align-items: center;
		  margin-bottom: 5px;
	    }
	}
</style>
