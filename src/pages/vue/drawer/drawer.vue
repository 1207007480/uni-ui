<template>
	<view>
		<view class="header">
			<view class="input-view">
				<uni-icons type="search" size="22" color="#666666" />
				<input confirm-type="search" class="input" type="text" placeholder="输入搜索关键词" @confirm="confirm">
			</view>
		</view>
		<text class="example-info">这是抽屉式导航组件使用示例，可以指定菜单左侧或者右侧弹出（仅初始化生效），组件内部可以放置任何内容。点击页面按钮即可显示导航菜单。</text>
		<view>
			<uni-section title="左侧滑出" type="line"></uni-section>
			<view class="example-body">
				<view class="word-btn draw-cotrol-btn" hover-class="word-btn--hover" :hover-start-time="20" :hover-stay-time="70"
				 @click="showDrawer('showLeft')"><text class="word-btn-white">显示Drawer</text></view>
				<uni-drawer ref="showLeft" mode="left" :width="320" @change="change($event,'showLeft')">
					<!-- #ifndef MP-BAIDU || MP-ALIPAY || MP-TOUTIAO -->
					<uni-list>
						<uni-list-item title="Item1" />
						<uni-list-item title="Item2" />
						<uni-list-item :show-badge="true" title="Item3" badge-text="12" />
					</uni-list>
					<!-- #endif -->
					<!-- #ifdef MP-BAIDU || MP-ALIPAY || MP-TOUTIAO -->
					<view class="uni-list">
						<uni-list-item title="Item1" />
						<uni-list-item title="Item2" />
						<uni-list-item :show-badge="true" title="Item3" badge-text="12" />
					</view>
					<!-- #endif -->
					<view class="close">
						<view class="word-btn" hover-class="word-btn--hover" :hover-start-time="20" :hover-stay-time="70" @click="closeDrawer('showLeft')"><text
							 class="word-btn-white">关闭Drawer</text></view>
					</view>
				</uni-drawer>
			</view>
			<uni-section title="右侧滑出" type="line"></uni-section>
			<view class="example-body">
				<view class="word-btn draw-cotrol-btn" hover-class="word-btn--hover" :hover-start-time="20" :hover-stay-time="70"
				 @click="showDrawer('showRight')"><text class="word-btn-white">显示Drawer</text></view>
				<uni-drawer ref="showRight" mode="right" :mask-click="false" @change="change($event,'showRight')">
					<view>
						<scroll-view class="scroll-view" scroll-y="true" >
							<view class="info">
								<text class="info-text">右侧遮罩只能通过按钮关闭，不能通过点击遮罩关闭</text>
							</view>
							<view class="close">
								<view class="word-btn" hover-class="word-btn--hover" :hover-start-time="20" :hover-stay-time="70" @click="closeDrawer('showRight')"><text
									 class="word-btn-white">关闭Drawer</text></view>
							</view>
							<view class="info-content" v-for="item in 100" :key="item">
								可滚动内容 {{item}}
							</view>
							<view class="close">
								<view class="word-btn" hover-class="word-btn--hover" :hover-start-time="20" :hover-stay-time="70" @click="closeDrawer('showRight')"><text
									 class="word-btn-white">关闭Drawer</text></view>
							</view>
						</scroll-view>
					</view>
				</uni-drawer>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				showRight: false,
				showLeft: false
			}
		},
		methods: {
			// 打开窗口
			showDrawer(e) {
				this.$refs[e].open()
			},
			// 关闭窗口
			closeDrawer(e) {
				this.$refs[e].close()
			},
			// 抽屉状态发生变化触发
			change(e, type) {
				console.log((type === 'showLeft' ? '左窗口' : '右窗口') + (e ? '打开' : '关闭'));
				this[type] = e
			}
		},
		onNavigationBarButtonTap(e) {
			this.showRight = !this.showRight
		},
		// app端拦截返回事件 ，仅app端生效
		onBackPress() {
			if (this.showRight || this.showLeft) {
				this.$refs.showLeft.close()
				this.$refs.showRight.close()
				return true
			}
		}
	}
</script>

<style lang="scss">
	@import '@/common/uni-nvue.scss';

	.header {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		padding: 10px 15px;
		align-items: center;
		border-top-width: 1px;
		border-top-color: #f5f5f5;
		border-top-style: solid;
		background-color: #ffffff;
	}

	.input-view {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		align-items: center;
		flex-direction: row;
		background-color: #e7e7e7;
		height: 30px;
		border-radius: 15px;
		padding: 0 10px;
		flex: 1;
		background-color: #f5f5f5;
	}

	.uni-drawer-info {
		background-color: #ffffff;
		padding: 15px;
		padding-top: 5px;
		color: #3b4144;
	}

	.uni-padding-wrap {
		padding: 0 15px;
		line-height: 1.8;
	}

	.input {
		flex: 1;
		padding: 0 5px;
		height: 24px;
		line-height: 24px;
		font-size: $uni-font-size-base;
		background-color: transparent;
	}

	.close {
		padding: 15px;
	}

	.example-body {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		padding: 0;
	}

	.draw-cotrol-btn {
		flex: 1;
	}

	.info {
		padding: 15px;
		color: #666;
	}

	.info-text {
		font-size: 14px;
		color: #666;
	}
	// 处理抽屉内容滚动
	.scroll-view {
		flex: 1;
		position: absolute;
		top: 0;
		bottom: 0;
	}
	.info-content {
		padding: 5px 15px;
	}
</style>
