<template>
	<view>

		<uni-section :title="instanceName" type="line"></uni-section>
		<view class="example-info">
			<text class="example-info-text"> {{enrollStateName}}</text>
		</view>
		<view class="bottomLine3" />
		
		<view>
			<view class="adBaseView" v-for="(item,i)  in resultList">
				<view class="cloumnlist">
					{{item}}
				</view>
				<view class="bottomLine2" />
			</view>
		</view>

		<view v-if="isExportPdf">
			<button class="button-cell" @click="donlowd">下载{{pdfName}}</button>
		</view>
		<view v-if="isExportPdf1">
			<button class="button-cell" @click="donlowd1">下载{{pdfName1}} </button>
		</view>
	</view>
</template>

<script>
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import {
		enrollProjectInstanceApply
	} from '@/api/menu.js'
	export default {
		components: {
			uniNoticeBar,
			uniSection
		},
		data() {
			return {
				isExportPdf: false,
				isExportPdf1: false,
				instanceId: '',
				retType: '',
				index: 0,
				templateNum:'1',
				pdfName:'',
				pdfName1:'',
				itemPersonId: '',
				instanceName: '',
				instanceDes: '',
				resultList: '',
				enrollStateName: '',
				itemList: []
			}
		},
		onLoad(options) {

			this.instanceId = options.instanceId
			enrollProjectInstanceApply({
				instanceId: this.instanceId
			}).then(res => {
				this.instanceDes = res.data.instanceDes
				this.instanceName = res.data.instanceName
				this.resultList = res.data.resultList
				this.retType = res.data.retType
				this.isExportPdf = res.data.isExportPdf
				this.itemPersonId = res.data.itemPersonId
				this.enrollStateName = res.data.enrollStateName
				this.isExportPdf1 = res.data.isExportPdf1
				this.pdfName = res.data.pdfName
				this.pdfName1 = res.data.pdfName1
			}).catch(err => {

			})
		},
		methods: {
			donlowd() {
				wx.downloadFile({
					url: getApp().globalData.enrollurl + '/enroll/downloadEnrollAppointment?itemPersonId=' + this.itemPersonId + '',
					header: {
						"Content-Type": "application/json",
						"Cookie": "JSESSIONID=" + getApp().globalData.vueSessionId
					},
					success: (res) => {
						if (res.statusCode === 200) {
							var filePath = res.tempFilePath;
							wx.openDocument({
								filePath: filePath,
								fileType: 'pdf',
								showMenu: true,
								success: function(res) {
									console.log('打开文档成功')
								},
								fail: function(res) {
									console.log(res);
								},
								complete: function(res) {
									console.log(res);
								}
							})
							uni.showToast({
								title: `下载成功`,
								icon: 'none'
							})
						}
					}
				});
			},
			donlowd1() {
				wx.downloadFile({
					url: getApp().globalData.enrollurl + '/enroll/downloadEnrollAppointment?itemPersonId=' + this.itemPersonId + '&&templateNum='+this.templateNum+'',
					header: {
						"Content-Type": "application/json",
						"Cookie": "JSESSIONID=" + getApp().globalData.vueSessionId
					},
					success: (res) => {
						if (res.statusCode === 200) {
							var filePath = res.tempFilePath;
							wx.openDocument({
								filePath: filePath,
								fileType: 'pdf',
								showMenu: true,
								success: function(res) {
									console.log('打开文档成功')
								},
								fail: function(res) {
									console.log(res);
								},
								complete: function(res) {
									console.log(res);
								}
							})
							uni.showToast({
								title: `下载成功`,
								icon: 'none'
							})
						}
					}
				});
			}
		}
	}
</script>

<style>
	.example-info {
		padding: 15px;
		color: black;
		font-weight: bold;
		text-align: center;
		background: #ffffff;
	}
	

</style>
