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
			Eyear: [
				{name:"2016-1",css:"active"},
			],
			Ayear: [
				{name:"2016",css:"active"},
			],
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



<!-- 无法引用less变量！
<style lang="less">
	@import url("@/common/uni.less");
</style> 
 -->
<style>
	@import "./common/psl_font.css";
</style>
