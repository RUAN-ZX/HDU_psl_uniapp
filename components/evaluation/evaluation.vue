<template>
	<view class="card top_card">
		<view class="label">
			{{e.Etime}} 学评教
			<span class="e_participate">
				{{title.e[3]}} 
				<span class="evalue">
					{{e.Eparticipate}}
				</span>
				
			</span>
		</view>
		
		<view class="div_1 ">
			<view class="score">
				{{title.e[0]}}
				
				<span class="scoreNum">
					{{e.Escore}}
				</span>
			</view>
			<view class="srank">
				{{title.e[1]}}
				<span class="value">{{e.Esrank}}</span>
			</view>

			<view class="prank">
				{{title.e[2]}}
				<span class="value">{{e.Eprank}}</span>

			</view>
		</view>
		
		<view 
			v-for="item in c"
			v-key="id"
			class="div_2">
			
			<u-collapse event-type="close" :head-style="head_style">
				<u-collapse-item :title="item.Cname">
					 
					<view class="ccid">
						{{item.CCid}}
					</view>
					<view class="evaluation_wrapper">
						<view class="evaluation">
								{{title.c[0]}}
							<span class="value">
								{{item.Cscore}}
							</span>
						</view>
						
						<view class="evaluation">
								{{title.c[1]}}
							<span class="value">
								{{item.Cparticipate}}
							</span>
						</view>
						
						<view class="evaluation">
								{{title.c[2]}}
							<span class="value">
								{{item.Cscore_1}}
							</span>
						</view>
						
						<view class="evaluation">
								{{title.c[3]}}
							<span class="value">
								{{item.Cscore_2}}
							</span>
						</view>
						
						<view class="evaluation">
								{{title.c[4]}}
							<span class="value">
								{{item.Cscore_3}}
							</span>
						</view>
						
						<view class="evaluation">
								{{title.c[5]}}
							<span class="value">
								{{item.Cscore_4}}
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
			title: {
				type: Object,
				default: function(){
					return {
						e:["总共得分","全校排名","本院排名","参评人次"],
						c:["总共得分","参评人数","教学能力","教学态度","师生交流","教学效果"],
						a:[
							{
								icon:"icon-grade",
								name:"考核等级"
							},
							{
								icon:"icon-time",
								name:"教学学时"
							},
							{
								icon:"icon-score",
								name:"考核分数"
							}
						]
					};
				}
			},
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
