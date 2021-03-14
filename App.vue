<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "./uview-ui/index.scss";
	@import "./uview-ui/theme.scss";
	
</style>
<script>
	export default {
		globalData: {  
			Tid:"",
			Tname:"",
			url :"http://localhost:8080",
			// url :"https://minipro.ryanalexander.cn",
			cap_info: {
				height: 0,
				top: 0,
				left: 0,
				right: 0,
				width: 0,
				bottom: 0
			},
			user: {
				name:"",
				avatar: "../../jpg/icon_div_2/1.jpg",
				phone:""
			},
			
			title: {
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
			},
			
		}, 
		onLaunch: function() {
			console.log('App Launch')
			this.globalData.cap_info.left = 0.1;
		},
		onShow: function() {
			console.log('App Show')
			let menuButton = uni.getMenuButtonBoundingClientRect();

			this.globalData.cap_info = {
			  height : menuButton.height,
			  right : menuButton.right,
			  left : menuButton.left,
			  top : menuButton.top,
			  width : menuButton.width,
			  bottom : menuButton.bottom
			};
		},
		onHide: function() {
			console.log('App Hide')
		},
		
		networkError:function(api,res){
		    console.log("Network fail "+api+" "+res.data.info);
		},
		serverError:function(api,res){
			console.log("server fail "+api+" "+res.data.info);
		},
	}
</script>


<style>
	@import "./common/psl_font.css";
	@import "./common/animate.css";
</style>
