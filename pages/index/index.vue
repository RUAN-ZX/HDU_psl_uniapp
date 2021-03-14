<template>
	<view>
		<view class="background" :style="{display:m_bg_hidden}">
		    <image 
		        class="preface" 
		        src="@/static/jpg/login/loading.gif"
		        mode="widthFix"
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

		    
		    <achievement :a="a"></achievement>
			<evaluation :e="e" :c="c"></evaluation>
			
		</view>
	</view>
</template>

<script>
	
	import achievement from "@/components/achievement/achievement.vue";
	import evaluation from "@/components/evaluation/evaluation.vue";
	import headerRyan from "@/components/header/header.vue";
	export default {
		components:{
			achievement,
			evaluation,
			headerRyan
		},
		data() {
			return {
				app: {},
				name:"",
				
				swiperList: [{
						image: 'http://www.hdu.edu.cn/uploads/images/20200423/202004231246411000.jpg',
						title: '昨夜星辰昨夜风，画楼西畔桂堂东'
					},
					{
						image: 'http://www.hdu.edu.cn/uploads/images/20190401/201904011018051000.jpg',
						title: '身无彩凤双飞翼，心有灵犀一点通'
					},
					{
						image: 'http://www.hdu.edu.cn/uploads/images/20190912/201909121632111000.jpg',
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
			loadingComplete(){
				this.m_hidden="";
				this.m_bg_hidden="none"
				
			}
		},
		onLoad: function (options){
			let _this = this;
			let access = uni.getStorageSync('a').toString();
			this.app = getApp().globalData;
			
			this.name = this.app.Tname;
			this.cap_info=this.app.cap_info;
			this.user=this.app.user;
			
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
			  },
			  complete: () => {
				 // _this.loadingComplete(); 
			  	
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
				  var a = [];
				  for (var _item of res.data.info){
				  a.push({
					name: _item.Cname,
					cid: _item.CCid,
					item:[
					  {
						name:_this.app.title.c[0],
						value: _item.Cscore
					  },
					  {
						name:_this.app.title.c[1],
						value: _item.Cparticipate
					  },
					  {
						name:_this.app.title.c[2],
						value:_item.Cscore_1
					  },
					  {
						name:_this.app.title.c[3],
						value:_item.Cscore_2
					  },
					  {
						name:_this.app.title.c[4],
						value:_item.Cscore_3
					  },
					  {
						name:_this.app.title.c[5],
						value:_item.Cscore_4
					  }
					]
				  })
				  }
				  _this.c = a;
				}
			  },
			  fail:function (res) {
				console.log("fail CgetLast years"+res);
			  },
			  complete: () => {
				// _this.loadingComplete(); 
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
					_this.e={
						elabel:res.data.info.Etime,
						srank:{
							value:res.data.info.Esrank,
							name:_this.app.title.e[0]
						},
						prank:{
							value:res.data.info.Eprank,
							name:_this.app.title.e[1]
						},
						score:{
							value:res.data.info.Escore,
							name:_this.app.title.e[2]
						},
						participate:{
							value: res.data.info.Eparticipate,
							name:_this.app.title.e[3]
						}
					}
				}
			  },
			  fail:function (res) {
				console.log("fail EgetLast years"+res);
			  },
			  complete: () => {
				_this.loadingComplete(); 
			  }
			})
		},
	}
</script>


<style lang="less">
	@import url("@/common/uni.less");
</style>