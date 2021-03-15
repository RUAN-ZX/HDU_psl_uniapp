<template>
	<view class="header" 
		:style="{paddingTop: top+'px'}" >
	    <view v-if="hasBack" class="back">
			<u-icon name="arrow-left"
			color="#f5f5f5" size="40" @click="navigateBack"></u-icon>
		</view>
		<view class="text"
	    :style="{marginLeft: left+'px'}">
	        {{title}}
	    </view>
	</view>
</template>

<script>
	export default {
		name: 'header-ryan',
		props: {
			hasBack:{
				type:Boolean,
				default: false
			},
			top:{
				type: String,
				default: "32"
			},
			left:{
				type: String,
				default: "10"
			},
			title: {
				type: String,
				default: ""
			}
		},
		data() {
			return {
				
			}
		},
		methods:{
			navigateBack: function(){
				uni.navigateBack({
					
				})
			}
		}
	}
</script>

<style scoped lang="less">
	@import "@/common/uni.less";
	.header{
	    position: relative;
	    z-index: 5;
	    padding: 10px;
	    width: 100%;
	    display: flex;
	    flex-direction: row;
	    background-color: @themeColor;
		color: @bgColor;
		
		font-size: 20px;
		.back{
			font-weight: bolder;
			width: 30px;
			text-align: center;
		}
	    .text{
			flex: 1;
	    }
	} 
</style>
