<script>
	export default {
		globalData: {
			vueSessionId: '',
			dateTime: new Date(),
			enrollurl: 'https://enroll.sdu.edu.cn/func',
			// enrollurl: 'http://localhost:8080/func'
		},
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	/*每个页面公共css */
.question {
		display: flex;
		flex-direction: row;
		margin-top: 10px;
		margin-bottom: 10px;

	}

	.question2 {
		display: flex;
		flex-direction: row;
		margin-left: 55px;
		font-size: 28upx;
	}
	.cloumnlist {
		text-align: center;
		/* margin-left: 30upx; */
		font-size: 28upx;
	}
  
	.fuwenben {
		border: 2upx solid #e64340;
		padding-top: 3upx;

		margin-left: 5upx;
		margin-right: 5upx;
	}

	.shuoming {
		width: 90%;
		font-size: 30upx;
		word-break: break-all;
		/*属性规定自动换行的处理方法。normal(使用浏览器默认的换行规则。),break-all(允许在单词内换行。),keep-all(只能在半角空格或连字符处换行。)*/
		text-overflow: ellipsis;
		display: -webkit-box;
		/** 对象作为伸缩盒子模型显示 **/
		-webkit-box-orient: vertical;
		/** 设置或检索伸缩盒对象的子元素的排列方式 **/
		-webkit-line-clamp: 3;
		/** 显示的行数 **/
		overflow: hidden;
		/** 隐藏超出的内容 **/
	}

	.adBaseView {
		width: 100%;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding-top: 20upx;
	}

	.adRowView {
		width: calc(100% - 48upx);
		display: flex;
		flex-direction: row;
		align-items: center;
		color: rgb(94, 94, 94);
	}
.checkBoxG{
	width: 100%;
	display: flex;
	background-color: #FFFFFF;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	font-size: 28upx;
	padding-top: 20upx;
}
	.headView {
		flex: 1;
		text-align: left;
		/* margin-left: 30upx; */
		font-size: 28upx;
		/*超出一行点点点*/
		overflow: hidden;
		/*超出的部分隐藏起来。*/
		white-space: nowrap;
		/*不显示的地方用省略号...代替*/
		text-overflow: ellipsis;
		/* 支持 IE */

		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.mustView {
		color: red;
		font-size: 32upx;
		margin-left: 10upx;
	}

	.bottomLine {
		width: 95%;
		height: 2upx;
		background-color: #DDDDDD;
		margin-top: 20upx;
	}

	.input {
		text-align: right !important;
		font-size: 28upx !important;
	}

	.input-text {
		text-align: right !important;
		font-size: 28upx !important;
		color: rgb(128, 128, 128);
	}

	.button-cell {

		width: 45%;
		font-size: 28upx;
		margin-top: 10upx;
		background-color: #e64340;

	}

	.button-cell2 {

		width: 45%;
		font-size: 28upx;
		margin-top: 10upx;
		background-color: #DD524D;

	}

	.bottomLine2 {
		width: 100%;
		height: 2upx;
		background-color: #DDDDDD;
		margin-top: 20upx;
	}

	.bottomLine3 {
		width: 100%;
		height: 2upx;
		background-color: #DDDDDD;
		margin-top: 2upx;
	}
	.title{
	  margin: 0 15upx;
	  color: black;
	  font-weight: bolder;
	  font-size: 30upx;
	  text-align: center;
	} 
	.content{
	 
	  padding: 10upx;
	  text-indent: 2em;
	  text-align: left;
	  margin: 10upx;
	  border: 2upx solid gray;
	  border-radius: 8upx;
	  background-color: white;
	}


	.record-item {
		margin: 5upx 15upx;
		padding: 15upx;
		background-color: white;
		border: 1upx solid gainsboro;
		border-radius: 8upx;
		box-shadow: 1upx 1upx 2upx #333333;
	}

	.f1 {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.condition {
		font-size: 30upx;
		color: black;
		font-weight: bold;
	}

	.date {
		font-size: 25upx;
	}

	.date2 {
		color: #e64340;
		font-size: 20upx;
	}
.date3 {
		color: #7acfa6;
		font-size: 20upx;
	}
	.address {
		margin-top: 5upx;
		font-size: 30upx;
	}
</style>
