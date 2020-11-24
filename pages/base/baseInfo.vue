<template>
	<view>
		<uni-section title="基本信息完善" type="line"/>
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">姓名</view>
				<view class="input-text">{{perName}}</view>
			</view>
			<view class="bottomLine"></view>
		</view>
		<view class="adBaseView">
			<view class="adRowView">
					<view class="headView">工号</view>
					<view class="input-text">{{perNum}}</view>
			</view>
			<view class="bottomLine"></view>
		</view>
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">
					<view class="mustView">*</view>性别</view>
				<view class="uni-list">
					<view class="uni-list-cell">
						<view class="uni-list-cell-db">
							<picker @change="bindPickerGenderChange" :value="genderIndex" :range="genders">
								<view class="uni-input">{{genders[genderIndex]}}</view>
							</picker>
						</view>
					</view>
				</view>
			</view>
			<view class="bottomLine"></view>
		</view>
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView"><view class="mustView">*</view>出生日期</view>
				<view>
					<picker mode="date" :value="perBirthday" :start="startDate" :end="endDate" @change="bindTimeChangePerBirth">
						<view>{{perBirthday}}</view>
					</picker>
				</view>
			</view>
			<view class="bottomLine"></view>
		</view>
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">学院</view>
				<view class="input-text">{{collegeName}}</view>
			</view>
			<view class="bottomLine"></view>
		</view>			
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">聘任类型</view>
				<view style="width: 70%;"><picker class="input" @change="bindchange" :value="index" :range="secondPerTypeList" :range-key="'label'">
					<view class="uni-input">{{secondPerTypeList[index].label}}</view>
					</picker></view>
			</view>
			<view class="bottomLine"/>
		</view>	
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">手机号</view>
				<view style="width: 70%;"><input  class="input" v-model="mobilePhone" placeholder="请输入手机号" /></view>
			</view>
			<view class="bottomLine"/>
		</view>			
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">银行名称</view>
				<view style="width: 70%;"><input  class="input" v-model="bankName" placeholder="请输入银行名称,可以为空" /></view>
			</view>
			<view class="bottomLine"/>
		</view>
			
		<view class="adBaseView">
			<view class="adRowView">
				<view class="headView">银行卡号</view>
				<view style="width: 70%;"><input  class="input" v-model="bankNo" placeholder="请输入银行卡号,可以为空" /></view>
			</view>
			<view class="bottomLine"/>
		</view>
		<view class="checkBoxG">
		<checkbox-group @click="clickRead">
			<label>
				<checkbox v-model="hasEnroll" />是否参加过研究生监考
			</label>
		</checkbox-group>
		<view class="bottomLine"/>
		</view>
		<button class="button-cell" @click="submit">保存并提交</button>
		<button class="button-cell2" @click="unboding">解除绑定</button>
	</view>
</template>

<script>
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import  { personBaseInfoMaintainInit } from '@/api/baseInfo.js'
	import  { personBaseInfoMaintain } from '@/api/baseInfo.js'
	import  { unbounding } from '@/api/login.js'
	export default {
		 components: {uniNoticeBar,uniSection},
		data() {
			return {
				perName: '',
				perNum: '',
				personId: '',
				index: 0,
				perIdCard:'',
				genderCode:'',
				genderName:'',
				perBirthday:'',
				hasEnroll:false,
				mobilePhone: '',
				bankNo: '',
				bankName: '',
				collegeName: '',
				secondPerType: '',
				secondPerTypeList: [],
				genderIndex: 0,
				genders: ['男', '女'],
				ProjectInstanceList: []
			}
		},
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			}
		},
		onShow(){
			
			personBaseInfoMaintainInit({}).then(res => {
							this.secondPerTypeList = res.data.secondPerTypeList
							this.perName = res.data.perName
							this.perNum = res.data.perNum
							this.index = res.data.secondPerTypeIndex
							this.personId = res.data.personId
							this.mobilePhone = res.data.mobilePhone
							this.bankNo = res.data.bankNo
							this.bankName = res.data.bankName
							this.collegeName = res.data.collegeName
							this.genderCode = res.data.genderCode
							this.genderName= res.data.genderName
							this.perIdCard = res.data.perIdCard
							if(res.data.perBirthday===undefined){
								var date = '1990-01-01'
								this.perBirthday = date
							}else{
								this.perBirthday = res.data.perBirthday
							}
							if (this.genderCode === '1') {
								this.genderIndex = 0
							} else {
								this.genderIndex = 1
							}
							this.hasEnroll = res.data.hasEnroll
							this.secondPerType = this.secondPerTypeList[this.index].value
						}).catch(err => {
							
						})
		},
		methods:{
			bindPickerGenderChange(e) {
				this.genderIndex = e.target.value
				if (this.genderIndex === '0')
					this.genderCode = '1';
				else
					this.genderCode = '2';
			},
			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
			
				if (type === 'start') {
					year = year - 100;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			bindchange(e){
				 this.index = e.target.value
				 
				this.secondPerType = this.secondPerTypeList[this.index].value
			},
			bindTimeChangePerBirth(e) {
				this.perBirthday = e.target.value
			},
			submit(){
				if(this.mobilePhone === ''||this.mobilePhone===undefined ||this.mobilePhone === 0||this.mobilePhone === '0'){
					uni.showModal({
					    title: '提示',
						showCancel: false,
						confirmColor: "#000000",
					    content: '手机号不能为空',
					    success: function (res) {
					       
					    }
					});
				}else if(this.secondPerType === '12' ||this.secondPerType === undefined || this.secondPerType === '13'||this.secondPerType === '14'||this.secondPerType === '21'||this.secondPerType === '31'){
					if(this.bankNo === ''|| this.bankNo ===undefined){
						uni.showModal({
						    title: '提示',
							showCancel: false,
							confirmColor: "#000000",
						    content: '银行卡号不能为空',
						    success: function (res) {
						       
						    }
						});
					}
					if(this.bankName === ''|| this.bankName === undefined){
						uni.showModal({
						    title: '提示',
							showCancel: false,
							confirmColor: "#000000",
						    content: '银行名称不能为空',
						    success: function (res) {
						       
						    }
						});
					}
				}
				else{
					personBaseInfoMaintain({personId:this.personId,secondPerType:this.secondPerType,
					mobilePhone:this.mobilePhone,bankNo:this.bankNo,bankName:this.bankName,
					perIdCard:this.perIdCard,perBirthday:this.perBirthday,genderCode:this.genderCode,
					hasEnroll:this.hasEnroll
					}).then(res => {
						if(res.re === 1){
							uni.showModal({
							    title: '提示',
								showCancel: false,
								confirmColor: "#000000",
							    content: '提交成功',
							    success: function (res) {
							        if (res.confirm) {
								          uni.switchTab({
								          	url:'../fist/fist'
								          })
							        } 
							    }
							});
						}
						
						}).catch(err => {
							
						})
				}
				},
				unboding(){
					unbounding({}).then(res => {
							uni.showModal({
							    title: '提示',
								showCancel: false,
								confirmColor: "#000000",
							    content: '解绑成功',
							    success: function (res) {
							        if (res.confirm) {
								          uni.navigateTo({
								          	url:'../index/index'
								          })
							        } 
							    }
							});
						}).catch(err => {
							
						})
				}
		}
}
</script>

<style>
</style>
