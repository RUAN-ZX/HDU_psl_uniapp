<template>
	<view class="card achievement">
	    <view class="label">
	        {{a.alabel}} 教学业绩考核
	        <span class="participate">
	            {{a.accident}}
	        </span>
	        <span class="text">
	            出现教学事故
	        </span>
	    </view>
	
	    <view class="content">
	        <view class="items_wrapper">
	            <view class="items"
	                v-for="(items,index) in a.aitem"
	                :key="index">
	                <view :class="'item_iconfont psl_font '+items.icon">
			    
	                </view>
	                
	                <view class="name">
	                    {{items.name}}
	                </view>
	                
	                <span class="value">
	                    {{items.value}}
	                </span>
	            </view>
			    
	            
	        </view>
	        <view class="info">
	            备注信息 <span class="value">
	                {{a.info}}
	            </span>
	        </view>
	    </view>
	</view>
</template>

<script>
	export default {
		name: 'achievement',
		props: {
			a:{
				type: Object,
				default: function(){
					return {
						alabel:"2018-2019",
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
					}
				}
			}
		},
		
		data() {
			return {
			};
		},
		created() {
			
		},
		methods:{
			
		}
	}
</script>

<style lang="less">
	@import "@/common/uni.less";
	@import "./achievement.less";
</style>
