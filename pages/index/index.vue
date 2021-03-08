<template>
	<view>
		<view class="background">
		    <image 
		        class="preface" 
		        src="@/static/jpg/login/loading.gif"
		        mode="widthFix"
		    >
		    </image>
		</view>
		<view :style="{display:m_hidden}" class="content">
		    <view class="header" :style="{paddingTop: cap_info.top+'px'}" >
		        <view class="text"
		        :style="{marginLeft: cap_info.height+'px'}">
		            {{name}}老师，欢迎您
		        </view>
		    </view>
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
	export default {
		components:{
			achievement,
			evaluation
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
				e:{
				  elabel:"2019-1 学评教",
				  srank:{
					name:"学校排名",
					value:"100"
				  },
				  prank:{
					name:"学院排名",
					value:"100"
				  },
				  score:{
					name:"总共得分",
					value:"100"
				  },
				  participate:{
					name:"参评人次",
					value:"100"
				  },
				},
				c:[],
				a:{
				  alabel:"2018-2019 教学业绩考核",
				  accident:"未", //已出现教学事故
				  info:"S3总分100分封顶，S4总分100分封顶", // 备注
				  aitem:[
					{
					  icon:"icon-grade",
					  name:"考核等级",
					  value:"A"
					},
					{
					  icon:"icon-time",
					  name:"教学学时",
					  value:"100"
					},
					{
					  icon:"icon-score",
					  name:"考核分数",
					  value:"100"
					}
				  ] 
				},
				m_hidden:"none",
				zIndex:10,
				cap_info:{},
			}
		},
		onLoad() {
			var _this = this;
			this.app = getApp().globalData;
		
			this.name = this.app.Tname;
			this.cap_info=this.app.cap_info;
			this.user=this.app.user;
			
			uni.request({
			  method:'post',
			  url: this.app.url+"/AgetLast", //仅为示例，并非真实的接口地址
			  data: {
				"ATid": this.app.Tid,
				"access": uni.getStorageSync('a')
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				  console.log("index");
				if(res.data.code==0){    
				    var temp = res.data.info;
				    _this.a={
					  alabel: temp.Atime+"-"+(temp.Atime+1),
					  accident: temp=="否"?"有":"未", //已出现教学事故
					  info: temp.Ainfo,
					  aitem:[
						{
						  icon:"icon-grade",
						  name:"考核等级",
						  value: temp.Agrade
						},
						{
						  icon:"icon-time",
						  name:"教学学时",
						  value: temp.Ahour
						},
						{
						  icon:"icon-score",
						  name:"考核分数",
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
				// console.log(1)
			  	_this.m_hidden="";
			  }
			})
		
			uni.request({
			  method:'post',
			  url: this.app.url+"/CgetLast", //仅为示例，并非真实的接口地址
			  data: {
				"CTid": this.app.Tid,
				"access": uni.getStorageSync('a')
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
						name:"总共得分",
						value: _item.Cscore
					  },
					  {
						name:"参评人数",
						value: _item.Cparticipate
					  },
					  {
						name:"教学能力",
						value:_item.Cscore_1
					  },
					  {
						name:"教学态度",
						value:_item.Cscore_2
					  },
					  {
						name:"师生交流",
						value:_item.Cscore_3
					  },
					  {
						name:"教学效果",
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
				  // console.log(2)
			  	_this.m_hidden="";
			  }
			})
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/EgetLast", //仅为示例，并非真实的接口地址
			  data: {
				"ETid": _this.app.Tid,
				"access": uni.getStorageSync('a')
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){
					_this.e.elabel = res.data.info.Etime;
					_this.e.srank.value=res.data.info.Esrank;
					_this.e.prank.value=res.data.info.Eprank;
					_this.e.score.value=res.data.info.Escore;
					_this.e.participate.value=res.data.info.Eparticipate;
				}
			  },
			  fail:function (res) {
				console.log("fail EgetLast years"+res);
			  },
			  complete: () => {
				  // console.log(3)
			  	_this.m_hidden="";
			  }
			})
			// console.log("EGetAllYears");
			uni.request({
			  method:'post',
			  url: _this.app.url+"/EGetAllYears", //仅为示例，并非真实的接口地址
			  data: {
				"ETid": _this.app.Tid,
				"access": uni.getStorageSync('a')
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				var a = [];
				for (var _item of res.data.info){
				  a.push({name:_item,css:""})
				}
				_this.app.Eyear = a;
			  },
			  fail:function (res) {
				console.log("fail EGetAll years"+res);
			  }
			});
			// console.log("AGetAllYears");
			uni.request({
			      method:'post',
			      url: _this.app.url+"/AGetAllYears", //仅为示例，并非真实的接口地址
			      data: {
			        "ATid": _this.app.Tid,
			        "access": uni.getStorageSync('a')
			      },
			      header: {
			          'content-type': 'application/x-www-form-urlencoded'
			      },
			      success: function(res) {
			        if(res.data.code==0){
			          var a = [];
			          for (var _item of res.data.info){
			            a.push({name:_item,css:""})
			          }
			          _this.app.Ayear = a;
			        }
			      },
			      fail:function (res) {
			        console.log("fail EGetAll years"+res);
			      }
			})
			
			
			
			
		},
		
	}
</script>


<style lang="less">
	@import url("@/common/uni.less");
	@import "./index.less";
</style>