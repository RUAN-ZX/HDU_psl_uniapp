<template>
	<view class="card top_card">
		<view class="label">
			{{e.elabel}} 学评教
			<span class="e_participate">
				{{e.participate.name}} 
				<span class="evalue">
					{{e.participate.value}}
				</span>
				
			</span>
		</view>
		<view class="div_1 ">
			<view class="score">
				{{e.score.name}}
				
				<span class="scoreNum">
					{{e.score.value}}
				</span>
			</view>
			<view class="srank">
				{{e.srank.name}}
				<span class="value">{{e.srank.value}}</span>
			</view>

			<view class="prank">
				{{e.prank.name}}
				<span class="value">{{e.prank.value}}</span>

			</view>
		</view>
		
		<view 
			v-for="item in c"
			v-key="id"
			class="div_2">
			
			<u-collapse event-type="close" :head-style="head_style">
				<u-collapse-item :title="item.name">
					 
					<view class="ccid">
						{{item.cid}}
					</view>
					<view class="evaluation_wrapper">
						<view 
							v-for="items in item.item"
							v-key="index"
							class="evaluation">
								{{items.name}}
							<span class="value">
								{{items.value}}
							</span>
						</view>
					</view>
				</u-collapse-item>
			</u-collapse>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'evaluation',
		props: {
			c:{
				type: Array,
				default: function(){
					return [];
				}
			},
			
			e:{
				type: Object,
				default: function(){
					return {
						elabel: "2019-1",
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
					}
				}
			}
		},
		
		data() {
			return {
				head_style:{
					fontWeight: 'bolder',
					height:"20px",
					lineHeight:"20px"
				}
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
	@import "./evaluation.less";
</style>
