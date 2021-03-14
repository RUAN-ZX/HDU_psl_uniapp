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
		
		<view class="content" :style="{display:m_hidden}">
			<header-ryan title="历年学评教及其明细"
				:top="cap_info.top"
				:left="cap_info.height">
				
			</header-ryan>
			<u-dropdown class="achievement" 
				ref="uDropdown" @open="open" @close="close" :border-bottom="true"
				>
				<u-dropdown-item 
					v-key="index"
					v-for="(item,index) in option"
					v-model="event[index].m" 
					:title="item[event[index].m-5*index].label" 
					:options="item" 
					@change="change">
				</u-dropdown-item>
			</u-dropdown>
			
			<!-- <view class="animate__animated" hover-class="tada"> -->

			<view 
				:class="ani" 
				v-if="ani_if"
				v-for="(item,index) in arrayCE">
				<evaluation :e="item" :c="item.Ec"></evaluation>
			</view>
			
			
		</view>
	</view>
</template>

<script>
	import achievement from "@/components/achievement/achievement.vue";
	
	import headerRyan from "@/components/header/header.vue";
	var _this;
	export default {
		components:{
			achievement,
			headerRyan
		},
		data() {
			return {
				ani_if: true,
				ani: "animated fadeInUp",
				animation: {},
				animationData: {},
				event: [{m:0},{m:5}], //0 1 2 || 3 4
				app:{},
				cap_info:{},
				option: [],
				arrayCE:[],
				m_hidden:"none",
				m_bg_hidden:""
			}
		},
		methods:{
			open(index) {
				// 展开某个下来菜单时，先关闭原来的其他菜单的高亮
				// 同时内部会自动给当前展开项进行高亮
				this.$refs.uDropdown.highlight();
			},
			close(index) {
				// 关闭的时候，给当前项加上高亮
				// 当然，您也可以通过监听dropdown-item的@change事件进行处理
				this.$refs.uDropdown.highlight(index);
			},
			loading: function(){
				this.m_hidden="none";
				this.m_bg_hidden="";
			},
			loadingComplete: function(){
				this.m_hidden="";
				this.m_bg_hidden="none";
			},
			sort_(e){
				// sorting the archievements
				let sort = e[1].m-5;
				let event = e[0].m;
				_this.arrayCE.sort(function(a,b){
					let result =0;
					switch(event){
						case 0:{
							result=a.Escore-b.Escore;
							break;
						}
						case 1:{
							result=a.Esrank-b.Esrank;
							break;
						}
						case 2:{
							result=a.Eprank-b.Eprank;
							break;
						}
						case 3:{
							result=a.Eparticipate-b.Eparticipate;
							break;
						}
						case 4:{
							result = b.Etime.localeCompare(a.Etime);
							if(sort==1&&result==1) return -1;
							else if(sort==1&&result<1) return 1;
							else return result;
							break;
						}
					}
					
					return sort ?result:(-result);
				})
				
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
			option_init: function(){
				let array_option = this.app.title.e;
				this.option = [
					[
						{
							label:array_option[0],
							value:0
						},
						{
							label:array_option[1],
							value:1
						},
						{
							label:array_option[2],
							value:2
						},
						{
							label:array_option[3],
							value:3
						},
						{
							label:"考核学期",
							value:4
						},
					],
					[
						{
							label:"由高到低",
							value:5
						},
						{
							label:"由低到高",
							value:6
						
						}
					]
				]
			}
		},
		
		onLoad: function () {
			_this = this;
			let access = uni.getStorageSync('a').toString();
			_this.app = getApp().globalData;
			_this.cap_info = _this.app.cap_info;

			_this.option_init();
			_this.sort_(_this.event);
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/CEGetAll", //仅为示例，并非真实的接口地址
			  data: {
				"Tid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){  
				    _this.arrayCE = res.data.info;
				}
			  },
			  fail:function (res) {
				console.log("fail CEgetAll years"+res);
			  },
			  complete: () => {
				 _this.loadingComplete(); 
			  }
			});
			
			
		}
	}
</script>

<style lang="less">
	@import url("@/common/uni.less");

	.achievement{
		z-index: 99;
	} 
</style>

<style>
	@import '@/common/animate.css';
</style>