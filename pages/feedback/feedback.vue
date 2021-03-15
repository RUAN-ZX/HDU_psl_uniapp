<template>
	<view>
		<header-ryan title="交流联系 反馈问题"
			:top="cap_info.top"
			:left="cap_info.height"
			:hasBack="true">
		</header-ryan>
		 
		<view class="content">
			
			<u-form>
				<u-form-item :leftIconStyle="{color: '#888', fontSize: '32rpx'}" :left-icon="left_icon" label-width="120" 
				:label-position="labelPosition" 
					label="反馈主题" prop="name">
					<u-input :border="border" 
					placeholder="请概括性的说明反馈的主题" 
					v-model="Tid" type="text"></u-input>
				</u-form-item>
				
				<u-form-item
					:left-icon="left_icon"
					label-width="120"
					:leftIconStyle="{color: '#888', fontSize: '32rpx'}" 
					:label-position="labelPosition" label="备注" prop="intro">
					<u-input type="textarea" :border="border" 
					placeholder="请将想反馈的事务细节娓娓道来" 
					v-model="Tcomment" />
				</u-form-item>
				
				<u-form-item
					:label-position="labelPosition"
					:left-icon="left_icon"
					label-width="120"
					:leftIconStyle="{color: '#888', fontSize: '32rpx'}" 
					label="附加材料" 
					prop="photo" >
					<view class="explaination">必要的话 您可以上传照片材料以增强说服力</view>
					<u-upload width="160" height="160"></u-upload>
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
	.explaination{
		margin-left: 18px;
		margin-top: -20px;
		display: block;
		font-size: 12px;
		color: @labelColor;
		
	}
</style>
