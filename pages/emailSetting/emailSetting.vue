<template>
	<view>
		<header-ryan title="邮箱设定 邮箱换绑"
			:top="cap_info.top"
			:left="cap_info.height"
			:hasBack="true">
		</header-ryan>
		
		<view class="u-tabs-box">
			<u-tabs-swiper activeColor="#298FFE" ref="tabs" 
				:list="tab_list" :current="current"
				@change="change" :is-scroll="false" swiperWidth="750">
			 </u-tabs-swiper>
		</view>
		<swiper class="swiper-box" :current="swiperCurrent" @transition="transition" @animationfinish="animationfinish">
			<swiper-item class="swiper-item">
				<scroll-view :show-scrollbar="false" :scroll-y="false" style="height: 100%;width: 100%;">
					<view class="page-box">
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
									label="您的新邮箱绑定" prop="name">
									
									<u-input
										:border="border"
										placeholder-class="password" 
										type="text" 
										placeholder="请输入您的新邮箱"
										v-model = "Temail"/>
										
									<view class="psw_captcha">
										<u-input
											:border="border"
											maxlength="6"
											class="psw_input"
											placeholder-class="password" 
											type="text" 
											placeholder="新邮箱接收到的验证码"
											v-model = "Tcaptcha"/>
										
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
								
								<u-form-item 
									:label-position="labelPosition"
									:left-icon="left_icon"
									label-width="120"
									:leftIconStyle="{color: '#888', fontSize: '32rpx'}" 
									label="您的凭证" 
									prop="photo" >
									<view class="explaination">请上传如教工校园卡照片或其他更高等级的证明材料</view>
									<u-upload width="160" height="160"></u-upload>
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
				</scroll-view>
			</swiper-item>
			<swiper-item class="swiper-item">
				<scroll-view :show-scrollbar="false" :scroll-y="false" style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
					<view class="page-box">
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
											placeholder="旧邮箱接收到的验证码"
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
									label="您的新邮箱绑定" prop="name">
									
									<u-input
										:border="border"
										placeholder-class="password" 
										type="text" 
										placeholder="请输入您的新邮箱"
										v-model = "Temail_new"/>
										
									<view class="psw_captcha">
										<u-input
											:border="border"
											maxlength="6"
											class="psw_input"
											placeholder-class="password" 
											type="text" 
											placeholder="新邮箱接收到的验证码"
											v-model = "Tcaptcha_new"/>
										
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
				</scroll-view>
			</swiper-item>
		</swiper>
		
		
		 
		
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
			Temail: "",
			Tcaptcha: "",
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
			
			
			tabsHeight: 0,
			dx: 0,
			m_height: "100px",
			scrollHeight: 0,
			current: 0, // tabs组件的current值，表示当前活动的tab选项
			swiperCurrent: 0, // swiper组件的current值，表示当前那个swiper-item是活动的
			tab_list: [{
				name: '旧邮箱验证',
				num: 1
			}, {
				name: '证件照验证',
				num: 1
			}],
			
			
		};
	},
	onLoad: function () {
		_this = this;
		_this.app = getApp().globalData;
		_this.cap_info = _this.app.cap_info;
		
	},
	
	methods: {
		change(index) {
			this.swiperCurrent = index;
			// this.current = index;
			// 这个不能加 否则有bug
			
		},
		transition({
			detail: {
				dx
			}
		}) {
			this.$refs.tabs.setDx(dx);
		},
		animationfinish({
			detail: {
				current
			}
		}) {
			this.$refs.tabs.setFinishCurrent(current);
			this.swiperCurrent = current;
			this.current = current;
		},
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
		margin: @padding;
		border-radius: @padding;
		border: 1px solid @labelColor;
		width: auto;
		padding: 0 @padding @padding @padding;
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
	
	.u-tabs-box {
		width: 100vw;
		position: position;
	}
	
	
	.swiper-box {
		height: 100vh;
		.swiper-item {
			height: 100%;
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

