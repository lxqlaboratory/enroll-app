<template>
	<view>
		<view v-show="showSecondType">
			<uni-notice-bar :text="secondTypeNotice"></uni-notice-bar>
		</view>
		<view v-show="showTrain">
			<uni-section title="请选择培训地点" type="line"></uni-section>
			<helang-checkbox ref="checkbox" @change="valueChange"></helang-checkbox>
		</view>
		<view v-show="showShuttle">
			<uni-section title="请选择班车" type="line"></uni-section>
			<helang-checkbox ref="checkbox2" @change="valueChange2"></helang-checkbox>
		</view>

		<view v-show="showHashEnroll">
			<uni-section title="是否参加过监考" type="line"></uni-section>
			<radio-group class="question" @change="radioChange">
				<label class="question2" v-for="(item, index) in items" :key="item.value">
					<view>
						<radio :value="item.value" :checked="index === current" color="#e64340" style="transform:scale(0.85)" />
					</view>
					<view>{{item.name}}</view>
				</label>
			</radio-group>
		</view>

		<view>
			<uni-section title="请选择报名地点" type="line"></uni-section>
			<view class="adBaseView" v-for="items in applyList" :key="items.itemId" @click="submit1(items.itemId,items.enrollMode,items.itemName)">
				<view class="cloumnlist">
					{{items.itemName}}
				</view>
				<view class="cloumnlist" v-for="index in items.auxiliaryList">
					{{index}}
				</view>
				<view class="cloumnlist">
					需要{{items.needCount}}人
				</view>
				<view class="bottomLine2" />
			</view>
		</view>


		<view v-show="isCanCandidate">
			<uni-section title="候选报名地点" type="line"></uni-section>
			<view class="adBaseView" v-for="items in candidateList" :key="items.itemId" @click="submit2(items.itemId,items.enrollMode,items.itemName)">
				<view class="cloumnlist">
					{{items.itemName}}
				</view>
				<view class="cloumnlist" v-for="index in items.auxiliaryList">
					{{index}}
				</view>
				<view class="cloumnlist">
					需要{{items.needCount}}人
				</view>
				<view class="bottomLine2" />
			</view>
		</view>
	</view>
</template>

<script>
	import uniNoticeBar from '@/components/uni-notice-bar/uni-notice-bar.vue'
	import uniSection from '@/components/uni-section/uni-section.vue'
	import {
		enrollProjectInstanceApply
	} from '@/api/menu.js'
	import helangCheckbox from "@/components/helang-checkbox/helang-checkbox.vue"
	import {
		enrollProjectInstanceItemSubmit
	} from '@/api/menu.js'
	export default {
		components: {
			"helang-checkbox": helangCheckbox,
			uniNoticeBar,
			uniSection
		},
		data() {
			return {
				showShuttle: false,
				showTrain: false,
				showHashEnroll: false,
				showSecondType: false,
				secondTypeNotice: '',
				instanceId: '',
				retType: '',
				hasEnroll: '',
				shuttleItemList: [],
				shuttleSelect: '',
				trainSelect: '',
				trainItemListList: [],
				isNeedCheck: false,
				isCanCandidate: false,
				candidateList: [],
				items: [{
						value: '1',
						name: '参加过'
					},
					{
						value: '0',
						name: '未参加过'
					},
				],
				applyList: []
			}
		},
		onLoad(options) {

			this.instanceId = options.instanceId

			enrollProjectInstanceApply({
				instanceId: this.instanceId
			}).then(res => {
				this.candidateList = res.data.candidateList
				this.applyList = res.data.applyList
				this.secondTypeNotice = res.data.secondTypeNotice
				this.shuttleItemList = res.data.shuttleItemList
				this.trainItemListList = res.data.trainItemListList
				this.isNeedCheck = res.data.isNeedCheck
				this.isCanCandidate = res.data.isCanCandidate
				this.showHashEnroll = res.data.showHashEnroll
				this.showSecondType = res.data.showSecondType
				if (this.shuttleItemList.length > 0) {
					this.showShuttle = true
				}
				if (this.trainItemListList.length > 0) {
					this.showTrain = true
				}
				this.$refs.checkbox.set({
					type: 'radio', // 类型：单选框
					column: 2, // 分列
					list: this.trainItemListList
				});
				this.$refs.checkbox2.set({
					type: 'radio', // 类型：单选框
					column: 2, // 分列
					list: this.shuttleItemList
				});
			}).catch(err => {

			})
		},
		methods: {
			valueChange(data) {
				this.trainSelect = data
			},
			valueChange2(data) {
				this.shuttleSelect = data
			},
			radioChange(e) {

				this.hasEnroll = e.target.value
			},
			submit1(itemId, enrollMode, itemName) {
				if ((this.trainSelect === '' || this.trainSelect === undefined) && this.showTrain === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择培训地点',
						success: function(res) {

						}
					});
				} else if ((this.shuttleSelect === '' || this.shuttleSelect === undefined) && this.showShuttle === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择班车',
						success: function(res) {

						}
					});
				} else if ((this.hasEnroll === '' || this.hasEnroll === undefined) && this.showHashEnroll === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择是否参加过监考',
						success: function(res) {

						}
					});
				} else {
					console.log(itemId)
					console.log(enrollMode)
					var that = this;
					uni.showModal({
						title: '是否报名',
						content: itemName,
						success: function(res) {
							if (res.confirm) {

								enrollProjectInstanceItemSubmit({
									itemId: itemId,
									enrollMode: enrollMode,
									isNeedCheck: that.isNeedCheck,
									trainSelect: that.trainSelect.text,
									hasEnroll: that.hasEnroll,
									shuttleSelect: that.shuttleSelect.text
								}).then(res => {
									if (res.re === 1) {
										uni.showModal({
											title: '提示',
											showCancel: false,
											confirmColor: "#000000",
											content: '报名成功',
											success: function(res) {
												if (res.confirm) {
													uni.switchTab({
														url: '../fist/fist'
													})
												}
											}
										});

									} else {
										uni.showModal({
											title: '提示',
											showCancel: false,
											confirmColor: "#000000",
											content: '报名已满',
											success: function(res) {
												if (res.confirm) {
													console.log(that.instanceId)
													uni.redirectTo({
														url: '../baoming/fillinfo?instanceId=' + that.instanceId + ''
													})
												}
											}
										});

									}

								}).catch(err => {

								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}


			},


			submit2(itemId, enrollMode, itemName) {

				if ((this.trainSelect === '' || this.trainSelect === undefined) && this.showTrain === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择培训地点',
						success: function(res) {

						}
					});
				} else if ((this.shuttleSelect === '' || this.shuttleSelect === undefined) && this.showShuttle === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择班车',
						success: function(res) {

						}
					});
				} else if ((this.hasEnroll === '' || this.hasEnroll === undefined) && this.showHashEnroll === true) {
					uni.showModal({
						title: '提示',
						showCancel: false,
						confirmColor: "#000000",
						content: '请选择是否参加过监考',
						success: function(res) {

						}
					});
				} else {

					console.log(itemId)
					console.log(enrollMode)
					var that = this;
					uni.showModal({
						title: '是否报名',
						content: itemName,
						success: function(res) {
							if (res.confirm) {

								enrollProjectInstanceItemSubmit({
									itemId: itemId,
									enrollMode: enrollMode,
									isNeedCheck: that.isNeedCheck,
									trainSelect: that.trainSelect.text,
									hasEnroll: that.hasEnroll,
									shuttleSelect: that.shuttleSelect.text
								}).then(res => {
									if (res.re === 1) {
										uni.showToast({
											title: '报名成功',
											duration: 2000

										});
										uni.switchTab({
											url: '../fist/fist'
										})
									} else {
										uni.showModal({
											title: '提示',
											showCancel: false,
											confirmColor: "#000000",
											content: '报名已满',
											success: function(res) {
												if (res.confirm) {
													console.log(that.instanceId)
													uni.redirectTo({
														url: '../baoming/fillinfo?instanceId=' + that.instanceId + ''
													})
												}
											}
										});

									}

								}).catch(err => {

								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				}

			}

		}
	}
</script>

<style>
</style>
