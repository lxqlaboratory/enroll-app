<template>
	<view>
		<view v-if="!showdetail">
			<view class="main-core">

				<view class="main-core-item" v-for="item in list" :key=item.resId>
					<view @click="swtich(item.cmd)">
						<image class="core-item-icon" :src="item.image"></image>
						<text class="core-item-name">{{item.name}}</text>
					</view>
				</view>

			</view>
		</view>
		<view v-if="showdetail">
			<uni-section title="校内报名" type="line" />
			<view v-if="showText">
				<uni-notice-bar :text="texta" />
				<button class="button-cell" @click="complete">完善个人信息</button>
			</view>
			<view v-if="!showyemian">
				<view v-for="items in ProjectInstanceList" :key="items.instanceId">

					<view class="record-item">
						<view class="f1" v-if="items.isCanEnroll===true" @click="entry(items.instanceId,items.isApply)">
							<view class="condition">{{items.instanceName}}</view>
							<view class="date" v-if="items.isApply===true">已报名</view>
							<view class="date" v-if="items.isApply===false">未报名</view>
						</view>
						<view class="f1" v-if="items.isCanEnroll===false">
							<view class="condition">{{items.instanceName}}</view>
							<view class="date" v-if="items.isApply===true">已报名</view>
							<view class="date" v-if="items.isApply===false">未报名</view>
						</view>
						<view class="f1" v-if="items.isApply===true">
							<view class="address" @click="entry(items.instanceId,items.isApply)">{{items.itemName}}(点击查看）</view>
							<view v-if="items.isCanEnroll===true">
								<view class="date2" v-if="items.isCandelete===true" @click="deleteItemPerson(items.instanceItemId)">删除报名</view>
							</view>
							<view class="date2" v-if="items.isCandelete===false">已通过</view>
						</view>
						<view v-if="items.isCanEnroll===true">
							<view class="f1" v-if="items.isApply===false" @click="entry(items.instanceId,items.isApply)">
								<view class="address"></view>
								<view class="date3">点击报名</view>
							</view>
						</view>

					</view>
				</view>

			</view>
			<!-- <view v-else>当前无报名信息</view> -->

		</view>
	</view>
</template>

<script>
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import {
		MenuList
	} from '@/api/login.js'
	import {
		getEnrollProjectInstanceList
	} from '@/api/login.js'
	import {
		deleteItemPerson
	} from '@/api/project.js'
	export default {
		components: {
			uniNoticeBar,
			uniSection
		},
		data() {
			return {
				texta: '',
				showText: false,
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
		onShow() {

			MenuList({}).then(res => {
				this.list = res.data
				if (this.list.length === 1) {
					this.showdetail = true
				} else {
					this.showdetail = false
				}
			}).catch(err => {

			})

			getEnrollProjectInstanceList({}).then(res => {
				if (res.re === -1) {
					this.showText = true
					this.texta = res.data
				} else {
					this.ProjectInstanceList = res.data.projectList
					this.retType = res.data.retType

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



		},
		methods: {
			swtich(url) {
				console.log(url)
				uni.navigateTo({
					url: '../enroll/' + url
				})
			},
			entry(instanceId, isApply) {
				if (isApply === true) {
					uni.navigateTo({
						url: '../baoming/showDetais?instanceId=' + instanceId + ''

					})
				} else if (isApply === false) {
					uni.navigateTo({
						url: '../baoming/baoming?instanceId=' + instanceId + ''
					})
				}

			},
			deleteItemPerson(instanceItemId) {

				deleteItemPerson({
					instanceItemId: instanceItemId
				}).then(res => {
					if (res.re === 1) {
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
			},
			complete() {
				uni.switchTab({
					url: '../base/baseInfo'
				})
			}
		}
	}
</script>

<style>
	.main-core {
		background-color: white;
		display: flex;
		flex-flow: row wrap;
		align-content: flex-start;
		height: 18%;
		overflow: hidden;

	}

	.main-core-item {
		display: flex;
		flex-flow: column wrap;
		justify-content: center;
		align-items: center;
		box-sizing: border-box;
		width: 20%;
		height: 170upx;
	}

	.core-item-icon {
		display: block;
		width: 85upx;
		height: 85upx;
		margin: 15upx auto;
	}

	.core-item-name {
		display: block;

		font-size: 20upx;
	}

	.title {
		position: relative;
		padding-bottom: 10upx;
		text-align: center;
		background-color: #C8C7CC;

	}

	.text {
		font-weight: 500;
		font-size: 30upx;

	}
</style>
