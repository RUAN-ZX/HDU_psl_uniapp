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
		
		<view class="content" :style="{display:m_hidden}">
			<header-ryan title="历年教学业绩考核"
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
					:title="item[event[index].m-4*index].label" 
					:options="item" 
					@change="change">
				</u-dropdown-item>
			</u-dropdown>
			
			<view 
				:class="ani"
				:style="{'animation-delay':index*0.1+'s'}"
				v-if="ani_if"
				v-for="(item,index) in array_a">
				<achievement :a="item"></achievement>
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
				ani: "",
				animation: {},
				animationData: {},
				event: [{m:0},{m:4}], //0 1 2 || 3 4
				app:{},
				cap_info:{},
				option: [],
				array_a:[],
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
				_this.fadeInUp();
			},
			sort_(e){
				// sorting the archievements
				// 此处代码不够简练 还需要修改achievement组件
				let sort = e[1].m-4;
				let event = e[0].m;
				if(event==0){
					_this.array_a.sort(function(a,b){
						let result = a.aitem[event].value.localeCompare(b.aitem[event].value)
						if(sort==1&&result==1) return -1;
						else if(sort==1&&result<1) return 1;
						else return result;
					})
				}
				else if(event==3){
					_this.array_a.sort(function(a,b){
						let result = b.alabel.localeCompare(a.alabel);
						
						if(sort==1&&result==1) return -1;
						else if(sort==1&&result<1) return 1;
						else return result;
					});
				}
				else{
					_this.array_a.sort(function(a,b){
						let result = a.aitem[event].value-b.aitem[event].value;
						return sort ?result:(-result);
					})
				}
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
				let array_option = this.app.title.a;
				this.option = [
					[
						{
							label:array_option[0].name,
							value:0
						},
						{
							label:array_option[1].name,
							value:1
						},
						{
							label:array_option[2].name,
							value:2
						},
						{
							label:"考核学期",
							value:3
						},
					],
					[
						{
							label:"由高到低",
							value:4
						},
						{
							label:"由低到高",
							value:5
						}
					]
				]
			}
		},
		
		onLoad: function () {
			_this = this;
			_this.loading();
			_this.app = getApp().globalData;
			let access = uni.getStorageSync('a').toString();
			
			_this.cap_info = _this.app.cap_info;

			_this.option_init();
			_this.sort_(_this.event);
			
			uni.request({
			  method:'post',
			  url: _this.app.url+"/AGetAll", //仅为示例，并非真实的接口地址
			  data: {
				"ATid": _this.app.Tid,
				"access": access
			  },
			  header: {
				  'content-type': 'application/x-www-form-urlencoded'
			  },
			  success: function(res) {
				if(res.data.code==0){    
				    var temp_array = res.data.info;
					for (let temp of temp_array) {
						// console.log(temp_array[temp]);
						_this.array_a.push({
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
						});
					}
					
				}
			  },
			  fail:function (res) {
				console.log("fail AgetAll years"+res);
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