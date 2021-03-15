<template>
	<view>
		<view class="background" style="display:none">
		    <image 
		        class="preface" 
		        src="https://stea.ryanalexander.cn/psl/loading.gif"
		        mode="widthFix"
				crossorigin="anonymous"
		    >
		    </image>
		</view>
		<view :style="{display:m_hidden}" class="content">
			<header-ryan :title="name+'老师，欢迎您'" 
				:top="cap_info.top"
				:left="cap_info.height">
				
			</header-ryan>
		   
			<u-swiper :list="swiperList" mode="round" 
				:effect3d="true"
				:title="true"
				duration="1000" :circular="true" :autoplay="true"
			>	
			</u-swiper>

		    <view @click="change" :class="ani" v-if="ani_if">
				<achievement :a="a"></achievement>
		
			</view>
		    
			<view 
				:class="ani" 
				v-if="ani_if"
				:style="{'animation-delay':0.1+'s'}">
				<evaluation
				:title="app.title" :e="e" :c="c"></evaluation>
			</view>
			
		</view>
	</view>
</template>

<script>
	
	import achievement from "@/components/achievement/achievement.vue";
	import evaluation from "@/components/evaluation/evaluation.vue";
	import headerRyan from "@/components/header/header.vue";
	var _this;
	export default {
		components:{
			achievement,
			evaluation,
			headerRyan
		},
		data() {
			return {
				ani: "",
				app: {},
				name:"",
				ani_if: true,
				swiperList: [{
						image: 'https://stea.ryanalexander.cn/psl/hdu1.jpg',
						title: '昨夜星辰昨夜风，画楼西畔桂堂东'
					},
					{
						image: 'https://stea.ryanalexander.cn/psl/hdu2.jpg',
						title: '身无彩凤双飞翼，心有灵犀一点通'
					},
					{
						image: 'https://stea.ryanalexander.cn/psl/hdu3.jpg',
						title: '谁念西风独自凉，萧萧黄叶闭疏窗'
					}
				],
				e:{},
				c:[],
				a:{},
				m_hidden:"none",
				m_bg_hidden: "",
				zIndex:10,
				cap_info:{}
			}
		},
		methods:{
			loading: function(){
				this.m_hidden="none";
				this.m_bg_hidden="";
			},
			loadingComplete: function(){
				this.m_hidden="";
				this.m_bg_hidden="none";
				_this.fadeInUp();
			},
			fadeInUp(){
				_this.ani = "animated fadeInUp";
			},
			change(e){
				_this.ani_if = false;
				_this.fadeInUp();
				_this.sort_(_this.event);
				_this.$nextTick(() => {
					_this.ani_if = true;
				});
				
			},
			test_timeout(){
				for (var i = 0; i < 50; i++) {
					let access = uni.getStorageSync('a').toString();
				}
				
				
			}
			
		},
		
		onLoad: function (options){
			_this = this;
			_this.loading();
			
			
			let access = uni.getStorageSync('a').toString();
			this.app = getApp().globalData;
			
			this.name = this.app.Tname;
			this.cap_info=this.app.cap_info;
			this.user=this.app.user;
			
			// this.test_timeout();
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/AGetLast", //仅为示例，并非真实的接口地址
			  data: {
				"ATid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){    
				    var temp = res.data.info;
				    _this.a={
					  alabel: temp.Atime+"-"+(temp.Atime+1),
					  accident: temp=="否"?"有":"未", //已出现教学事故
					  info: temp.Ainfo,
					  aitem:[
						{
						  icon: _this.app.title.a[0].icon,
						  name: _this.app.title.a[0].name,
						  value: temp.Agrade
						},
						{
						  icon: _this.app.title.a[1].icon,
						  name: _this.app.title.a[1].name,
						  value: temp.Ahour
						},
						{
						  icon: _this.app.title.a[2].icon,
						  name: _this.app.title.a[2].name,
						  value: temp.Ascore
						}
					  ]
					}
				}
			  },
			  fail:function (res) {
				console.log("fail AgetLast years"+res);
			  }
			});
		
		
			uni.request({
			  method:'post',
			  url: _this.app.url+"/CGetLast", //仅为示例，并非真实的接口地址
			  data: {
				"CTid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
				  
				  _this.c = res.data.info;
				}
			  },
			  fail:function (res) {
				console.log("fail CgetLast years"+res);
			  }
			});
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/EGetLast", //仅为示例，并非真实的接口地址
			  data: {
				"ETid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
					_this.e=res.data.info;
					_this.$nextTick(() => {
						_this.loadingComplete(); 
					});
					
				}
				
			  },
			  fail:function (res) {
				console.log("fail EgetLast years"+res);
			  }
			})
		},
		
	}
</script>


<style lang="less">
	@import url("@/common/uni.less");
</style>

