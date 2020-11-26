<template>
	<view>
	<uni-section title="校内报名" type="line"/>
			<view v-if="!showyemian" >
			  <view v-for="items in ProjectInstanceList" :key="items.instanceId" >
				 
				<view class="record-item" >
					<view class="f1" @click="entry(items.instanceId,items.isApply)">
						<view class="condition">{{items.instanceName}}</view>
						<view class="date" v-if="items.isApply===true">已报名</view>
						<view class="date" v-if="items.isApply===false">未报名</view>
					</view>
					<view class="f1" v-if="items.isApply===true">
						<view class="address" @click="entry(items.instanceId,items.isApply)">{{items.itemName}}(点击查看）</view>
						<view class="date2" v-if="items.isCandelete===true" @click="deleteItemPerson(items.instanceItemId)">删除报名</view>
						<view class="date2" v-if="items.isCandelete===false">已通过</view>
					</view>
					<view class="f1" v-if="items.isApply===false" @click="entry(items.instanceId,items.isApply)">
						<view class="address"></view>
						<view class="date3" >点击报名</view>
					</view>
				</view>
				
				
			  </view>
			    </view>
				<view v-else>当前无报名信息</view>
				
			</view>
		</view>
</template>

<script>
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import  { getEnrollProjectInstanceList } from '@/api/login.js'
	import {
		deleteItemPerson
	} from '@/api/project.js'
	export default {
		 components: {uniNoticeBar,uniSection},
		data() {
			return {
				showyemian: false,
				showdetail: false,
				loginName: '',
				list: [],
				instanceId: '',
				instanceName: '',
				retType: '',
				ProjectInstanceList: []
			}
		},
		onLoad() {
			
			getEnrollProjectInstanceList({}).then(res => {
							 this.ProjectInstanceList = res.data.projectList
							this.retType = res.data.retType
							
							if(this.retType === 1){
								this.showyemian = true;
							}
							if(this.ProjectInstanceList.length === 1){
		// 						if(this.ProjectInstanceList[0].isApply === true)
		// 						{
		
		// 							uni.navigateTo({
		// 								url:'../baoming/showDetais?instanceId='+this.ProjectInstanceList[0].instanceId+''
		// 							})
		// 						}else if(this.ProjectInstanceList[0].isApply === false){
		// 							uni.navigateTo({
		// 								url:'../baoming/baoming?instanceId='+this.ProjectInstanceList[0].instanceId+''
		// 							})
		// 						}
							}
						}).catch(err => {
							
						})
		},
		methods:{
			entry(instanceId,isApply){
				if(isApply === true)
				{
					uni.navigateTo({
						url:'../baoming/showDetais?instanceId='+instanceId+''
						
					})
				}else if(isApply === false){
					uni.navigateTo({
						url:'../baoming/baoming?instanceId='+instanceId+''
					})
				}
				
			},
			deleteItemPerson(instanceItemId) {
			
			deleteItemPerson({instanceItemId:instanceItemId}).then(res => {
			       if(res.re === 1){
					   getEnrollProjectInstanceList({}).then(res2 => {
					   	if (res2.re === -1) {
					   		this.showText = true
					   		this.texta = res2.data
					   	} else {
					   		this.ProjectInstanceList = res2.data.projectList
					   		this.retType = res2.data.retType
					   
					   		if (this.retType === 1) {
					   			this.showyemian = true;
					   			this.showText = false;
					   		} else if (this.retType === 2) {
					   			this.showText = false;
					   		}
					   
					   
					   		if (this.ProjectInstanceList.length === 1) {
					   
					   		}
					   	}
					   
					   }).catch(err => {
					   
					   })
				   }
			}).catch(err => {
			
			})
			}
		}
	}
</script>

<style>
</style>
