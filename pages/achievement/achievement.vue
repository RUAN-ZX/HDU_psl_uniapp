<template>
	<view>
		<view class="background" :style="{display:m_hidden}">
		    <image 
		        class="preface" 
		        src="@/static/jpg/login/loading.gif"
		        mode="widthFix"
		    >
		    </image>
		</view>
		
		<view class="content">
		    <view 
		        class="shield"
		        :style="[{zIndex:z_shield},{backgroundColor: opacity_shield}]"
		        @tap = "shield"
		    >   
		    </view>
		    
		    <view class="header" :style="{paddingTop: cap_info.top+'px'}">
		        <view class="text"
					:style="{marginLeft:cap_info.height+'px'}">
		            <view class="years" @tap="choiceChange_1">
		                <span class="years-name">
							{{combobox.name}}
						</span>
		                <span class="years-icon psl_font icon-down">
		    
		                </span>
		            </view>
		            教学业绩考核
		        </view>
		    </view>
		    
		    <view :class="'choice '+hide.choice_1">
		        <view 
		            :class="'title '+item_title.css" 
		            v-for="(item_title,index_title) in combobox.child"
		            :key="index_title"
		            @tap = "filter"
					data-type="0"
					:data-choice = "index_title"
		            >
		            {{item_title.name}}
		        </view>
		    </view>
		    
			<achievement :a="a"></achievement>
			
		</view>
	</view>
</template>

<script>
	import achievement from "@/components/achievement/achievement.vue";
	export default {
		components:{
			achievement
		},
		data() {
			return {
				app:{},
				hide:{
					choice_1:"choice_hide",
				},
				cap_info:{},
				combobox:{},
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
				opacity_shield:"transparent",
				z_shield:0,
				scrollEnable:"relative",
				m_hidden:"",
			}
		},
		methods:{
			shield:function(e) {
				let type = parseInt(e.currentTarget.dataset.type);
				
				this.hide.choice_1="choice_hide"; 
		
				this.opacity_shield="#00000020";
				this.z_shield=0;
				this.scrollEnable="relative";
				
			},
			filter:function(e){
				
				let _combobox = this.combobox;
				let last_index = this.combobox.last_index;
				let choice = parseInt(e.currentTarget.dataset.choice);
				_combobox.child[last_index].css = "";
				_combobox.child[choice].css="active";
				_combobox.name = _combobox.child[last_index].name;
		
				_combobox.last_index = choice;
				this.combobox = _combobox;
				this.choiceChange_1();
			},
			choiceChange_1:function () {
				// console.log("change_1",this.hide.filter,this.hide.choice_1,this.hide.choice_2,this.hide.choice_3);
				if(this.hide.choice_1=="choice_hide"){
					this.opacity_shield="#00000010";
					this.z_shield=3;
					this.scrollEnable="fixed";
		
					this.hide.choice_1="";
				} 
				else if(this.hide.choice_1==""){
					this.hide.choice_1="choice_hide";
					this.opacity_shield="transparent";
					this.z_shield=0;
					this.scrollEnable="relative";
					
				} 
			},
		},
		onLoad: function () {
			var _this = this;
			this.app = getApp().globalData;
		      let _combobox = {
		        last_index: 0,
		        name: "",
		        child: this.app.Ayear
		      };
			  console.log(_combobox);
		      let index = _combobox.last_index;
		      _combobox.name = _combobox.child[index].name;
		      _combobox.child[index].css = "active";
		
		      uni.request({
		        method:'post',
		        url: _this.app.url+"/AGetOne", 
		        data: {
		          "ATid": _this.app.Tid,
		          "year": _combobox.child[index].name,
		          "access": uni.getStorageSync('a')
		        },
		        header: {
		          'content-type': 'application/x-www-form-urlencoded'
		        },
		        success: function(res) {
		            if(res.data.code==0){    
		                let temp = res.data.info;
		                _this.a={
		                    alabel: temp.Atime+"-"+(temp.Atime+1)+" 教学业绩考核",
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
		        }
			})
		
			this.cap_info = this.app.cap_info;
			this.combobox=_combobox;
			// this.animation = uni.createAnimation();
		}, 
		onReady() {
			this.m_hidden="none";
		}
	}
</script>

<style lang="less">
	@import url("@/common/uni.less");
	@import "./achievement.less";
</style>
