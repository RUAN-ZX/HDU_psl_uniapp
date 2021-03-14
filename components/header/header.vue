<template>
	<view class="header" :style="{paddingTop: top+'px'}" >
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
	    .text{
	        color: @bgColor;
	        font-size: 20px;
	
	    }
	} 
</style>
