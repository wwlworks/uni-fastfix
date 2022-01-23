<template>
	<view
		v-if="visibleSync"
		:style="[
			customStyle,
			popupStyle,
			{
				zIndex: uZindex - 1
			}
		]"
		class="u-drawer"
		hover-stop-propagation
	>
		<wxy-mask
			:duration="duration"
			:custom-style="maskCustomStyle"
			:maskClickAble="maskCloseAble"
			:z-index="uZindex - 2"
			:show="showDrawer && mask"
			@click="maskClick"
		></wxy-mask>

		<view
			class="u-drawer-content"
			@tap="modeCenterClose(mode)"
			:class="[
				safeAreaInsetBottom ? 'safe-area-inset-bottom' : '',
				'u-drawer-' + popMode,
				showDrawer ? 'u-drawer-content-visible' : '',
				zoom && popMode == 'center' ? 'u-animation-zoom' : ''
			]"
			@touchmove.stop.prevent
			@tap.stop.prevent
			:style="[fullscreen ? maxStyle : style]"
		>
			<template v-if="popMode == 'center'">
				<view class="u-mode-center-box" @tap.stop.prevent @touchmove.stop.prevent :style="[fullscreen ? maxStyle : centerStyle]">
					<view class="u-menu" :class="['u-menu--' + menuPosition]" v-if="menuable">
						<template v-if="menuFull">
							<wxy-icon :name="fullscreenIconPath[1]" :color="fullscreenIconColor" size="0.9em" @click="bindexitFullScreen" v-if="fullscreen"></wxy-icon>
							<wxy-icon :name="fullscreenIconPath[0]" :color="fullscreenIconColor" size="0.9em" @click="bindFullscreen" v-else></wxy-icon>
						</template>
						<wxy-icon :name="closeIcon" :color="closeIconColor" size="0.9em" @click="close"></wxy-icon>
					</view>
					<scroll-view class="u-drawer__scroll-view" scroll-y="true" :style="[fullscreen ? maxScrollStyle : scrollStyle]"><slot /></scroll-view>
				</view>
			</template>

			<template v-else>
				<view class="u-menu" :class="['u-menu--' + menuPosition]" v-if="menuable">
					<wxy-icon :name="closeIcon" :color="closeIconColor" size="0.9em" @click="close"></wxy-icon>
				</view>
				<scroll-view class="u-drawer__scroll-view" scroll-y="true" :style="[fullscreen ? maxScrollStyle : scrollStyle]"><slot /></scroll-view>
			</template>
		</view>
	</view>
</template>

<script>
/**
 * popup 弹窗(基于uViewUI改进增强)
 * @description 弹出层容器，用于展示弹窗、信息提示等内容，支持上、下、左、右和中部弹出。组件只提供容器，内部内容由用户自定义
 * @property {String}  mode  弹出方向（默认left）
 * @property {Boolean} mask  是否显示遮罩（默认true）
 * @property {Numberr | String} border-radius 弹窗圆角值（默认0）
 * @property {Boolean} mask-close-able        点击遮罩是否可以关闭弹出层（默认true）,，前提是mask是true
 *
 * @property {Boolean} menuable         是否显示菜单图标（默认false）
 * @property {Boolean} menuFull  全屏和恢复图标是否显示,默认是false,它作用前提是menuable是true
 * @property {String}  menu-position    菜单图标位置,可选left,center,right，默认是right;
 * @property {Boolean} fullScreen       是否全屏,默认是false
 * @property {String}  close-icon       关闭图标的名称，指定名称由https://remixicon.com/中而来
 * @property {String}  close-icon-color 关闭图标的颜色（默认#909399）
 * @property {Array}   fullscreen-icon  全屏和恢复图标的名称，指定名称由https://remixicon.com/中而来，要注意是成对设置,用逗号分隔
 * @property {String}  fullscreen-icon-color 全屏和恢复图标的颜色（默认#909399）
 *
 * @property {Object} custom-style 用户自定义样式
 * @property {Stringr | Number} negative-top 中部弹出时，往上偏移的值
 * @property {Numberr | String} z-index 弹出内容的z-index值（默认1075）
 * @property {Boolean} zoom  是否开启缩放动画，只在mode为center时有效（默认true）
 * @property {Boolean} safe-area-inset-bottom 是否开启底部安全区适配（默认false）
 *
 * @event {Function} open 弹出层打开
 * @event {Function} close 弹出层收起
 *
 * 增强说明
 * @property {String} popColor          弹出内容的颜色，默认是白色,若为initial则透明，支持用户的png透明图片了（2021-1-7）
 * @property {String} min-max-width     最小最大宽度(逗号分隔，第一个是最小值，第二个最大值)（2021-06-15）
 * @example min-max-width="30vw"        最小宽度30vw
 * @example min-max-width="30vw,70vw"   最小宽度30vw,最大宽度70vw
 * @example min-max-width=",80vw"       最大宽度80vw
 * @property {String} min-max-height 最小最大高度(逗号分隔，第一个是最小值，第二个最大值)（2021-06-15）
 * @example min-max-height="30vh"       最小高度30vh
 * @example min-max-height="30vh,70vh"  最小高度30vh,最大高度70vh
 * @example min-max-height=",80vh"      最大高度80vh
 * @property {String} distance          上下左右弹窗时与边缘距离，默认是0，单位是自适应，若是数值字符则单位是rpx
 * @property {String} box-shadow        遮罩层隐藏状态下弹窗阴影,默认是'0 0 8px 4px #333333'
 *
 * 修改说明
 * 1、修改中心弹窗的最小宽度和最小高度，默认是10vw和10vh，若是使用过程中存在多余时，可通过min-max-width和min-max-height调整大小（2021-08-06）
 * 2、默认宽度和高度:均是fit-content，以内容为依据决定宽度和高度（2021-08-06）
 * 3、菜单图标增加了全屏和恢复，默认位置是right右侧,可选left和center（2021-08-06）
 * 4、上下左右弹窗时可指定距离边缘的距离，应用场景更灵活了，如弹窗中放个音乐播放，这样就可以是悬浮式音乐播放了，其它可以自由发挥了（2021-08-03）
 * 5、当遮罩层隐藏后，增加了事件穿透功能，可点击弹窗以外区域事件（2021-08-03）
 */

let windowWidth, windowHeight;
export default {
	props: {
		//显示状态
		show: {
			type: Boolean,
			default: false
		},
		// 弹出方向，left|right|top|bottom|center
		mode: {
			type: String,
			default: 'left'
		},
		// 是否显示遮罩
		mask: {
			type: Boolean,
			default: true
		},
		// 是否开启缩放动画，只在mode=center时有效
		zoom: {
			type: Boolean,
			default: true
		},
		// 是否开启底部安全区适配，开启的话，会在iPhoneX机型底部添加一定的内边距
		safeAreaInsetBottom: {
			type: Boolean,
			default: false
		},
		// 是否可以通过点击遮罩进行关闭，前提是mask是true
		maskCloseAble: {
			type: Boolean,
			default: true
		},
		// 用户自定义样式
		customStyle: {
			type: Object,
			default() {
				return {};
			}
		},
		value: {
			type: Boolean,
			default: false
		},
		// 此为内部参数，不在文档对外使用，为了解决Picker和keyboard等融合了弹窗的组件
		// 对v-model双向绑定多层调用造成报错不能修改props值的问题
		popup: {
			type: Boolean,
			default: true
		},
		// 显示显示弹窗的圆角，单位rpx
		borderRadius: {
			type: [Number, String],
			default: 0
		},
		zIndex: {
			type: [Number, String],
			default: ''
		},
		// 是否显示菜单
		menuable: {
			type: Boolean,
			default: false
		},
		//是否显示全屏和恢复图标
		menuFull: {
			type: Boolean,
			default: false
		},
		// 菜单位置,默认是right，可选left、center和right
		menuPosition: {
			type: String,
			default: 'right'
		},
		//是否全屏
		fullScreen: {
			type: Boolean,
			default: false
		},
		// 关闭图标的名称，只能uView的内置图标（原作废，改为由https://remixicon.com/中而来）
		closeIcon: {
			type: String,
			default: 'close-line'
		},
		// 关闭图标的颜色
		closeIconColor: {
			type: String,
			default: '#909399'
		},
		// 全屏图标的名称，来自https://remixicon.com/，第一个是全屏，第二个是退出全屏
		fullscreenIcon: {
			type: Array,
			default() {
				return ['fullscreen-line', 'fullscreen-exit-line'];
			}
		},
		// 关闭图标的颜色
		fullscreenIconColor: {
			type: String,
			default: '#909399'
		},
		// 宽度，只对左，右，中部弹出时起作用，单位rpx，或者"auto"
		width: {
			type: String,
			default: 'fit-content'
		},
		// 高度，只对上，下，中部弹出时起作用，单位rpx，或者"auto"
		height: {
			type: String,
			default: 'auto'
		},
		// 给一个负的margin-top，往上偏移，避免和键盘重合的情况，仅在mode=center时有效
		negativeTop: {
			type: [String, Number],
			default: 0
		},
		// 遮罩的样式，一般用于修改遮罩的透明度
		maskCustomStyle: {
			type: Object,
			default() {
				return {};
			}
		},
		// 遮罩打开或收起的动画过渡时间，单位ms
		duration: {
			type: [String, Number],
			default: 250
		},
		// 弹出内容颜色，默认是白色,若是透明transparent则直接显示内容背景
		popColor: {
			type: String,
			default: '#ffffff'
		},
		// 最小最大宽度(逗号分隔，第一个是最小值，第二个最大值)
		minMaxWidth: {
			type: String,
			default: ','
		},
		// 最小最大高度(逗号分隔，第一个是最小值，第二个最大值)
		minMaxHeight: {
			type: String,
			default: ','
		},
		//上下左右弹窗离边缘距离,默认单位是rpx，支持px,rpx,em,vw,vh等相对单位
		distance: {
			type: String,
			default: '0'
		},
		//弹窗阴影,只在遮罩层隐藏状态下有效
		boxShadow:{
			type: String,
			default: '0 0 8px 4px #333333'
		}
	},
	data() {
		return {
			visibleSync: false,
			showDrawer: false,
			timer: null,
			closeFromInner: false, // value的值改变，是发生在内部还是外部
			fullscreen: false
		};
	},
	computed: {
		popupStyle() {
			let style = {};
			//如果隐藏遮罩层则允许事件穿透
			if (!this.mask) style.pointerEvents = 'none';
			return style;
		},
		// 根据mode的位置，设定其弹窗的宽度(mode = left|right)，或者高度(mode = top|bottom)
		style() {
			let style = {};
			// 如果是左边或者上边弹出时，需要给translate设置为负值，用于隐藏
			if (this.mode.toLowerCase() == 'left' || this.mode.toLowerCase() == 'right') {
				style = {
					width: this.width ? this.addUnit(this.width) : 'fit-content',
					height: this.height ? this.addUnit(this.height) : 'fit-content',
					transform: `translate3D(${this.mode.toLowerCase() == 'left' ? '-100%' : '100%'},0px,0px)`
				};
				if (this.mode.toLowerCase() == 'left') style.left = this.distance ? this.addUnit(this.distance) : '0';
				if (this.mode.toLowerCase() == 'right') style.right = this.distance ? this.addUnit(this.distance) : '0';
				style.backgroundColor = this.popColor;
				// 容器内边距
				if (!this.menuable) style.padding = '0.1em 0.2em';
				else style.padding = '0 0.2em';
				if (!this.mask) style.boxShadow =this.boxShadow;
			}
			if (this.mode.toLowerCase() == 'top' || this.mode.toLowerCase() == 'bottom') {
				style = {
					width: this.width ? this.addUnit(this.width) : 'fit-content',
					height: this.height ? this.addUnit(this.height) : 'fit-content',
					transform: `translate3D(0px,${this.mode.toLowerCase() == 'top' ? '-100%' : '100%'},0px)`
				};
				if (this.mode.toLowerCase() == 'top') style.top = this.distance ? this.addUnit(this.distance) : '0';
				if (this.mode.toLowerCase() == 'bottom') style.bottom = this.distance ? this.addUnit(this.distance) : '0';
				style.backgroundColor = this.popColor;
				// 容器内边距
				if (!this.menuable) style.padding = '0.1em 0.2em';
				else style.padding = '0 0.2em';
				if (!this.mask) style.boxShadow =this.boxShadow;
			}

			style.zIndex = this.uZindex;
			// 如果用户设置了borderRadius值，添加弹窗的圆角
			if (this.borderRadius) {
				if (this.distance != '0') {
					style.borderRadius = `${this.addUnit(this.borderRadius)}`;
				} else {
					switch (this.mode.toLowerCase()) {
						case 'left':
							style.borderRadius = `0 ${this.addUnit(this.borderRadius)} ${this.addUnit(this.borderRadius)} 0`;
							break;
						case 'top':
							style.borderRadius = `0 0 ${this.addUnit(this.borderRadius)} ${this.addUnit(this.borderRadius)}`;
							break;
						case 'right':
							style.borderRadius = `${this.addUnit(this.borderRadius)} 0 0 ${this.addUnit(this.borderRadius)}`;
							break;
						case 'bottom':
							style.borderRadius = `${this.addUnit(this.borderRadius)} ${this.addUnit(this.borderRadius)} 0 0`;
							break;
						default:
					}
				}
				// 不加可能圆角无效
				style.overflow = 'hidden';
			}

			if (this.duration) style.transition = `all ${this.duration / 1000}s linear`;
			// 加入最小和最大高度或宽度
			let minMaxWidth = this.minMaxWidth.split(',');
			if (minMaxWidth.length == 1 && minMaxWidth[0] != '') style.minWidth = this.addUnit(minMaxWidth[0]);
			if (minMaxWidth.length == 2) {
				if (minMaxWidth[0] != '') style.minWidth = this.addUnit(minMaxWidth[0]);
				if (minMaxWidth[1] != '') style.maxWidth = this.addUnit(minMaxWidth[1]);
			}
			let minMaxHeight = this.minMaxHeight.split(',');
			if (minMaxHeight.length == 1 && minMaxHeight[0] != '') style.minHeight = this.addUnit(minMaxHeight[0]);
			if (minMaxHeight.length == 2) {
				if (minMaxHeight[0] != '') style.minHeight = this.addUnit(minMaxHeight[0]);
				if (minMaxHeight[1] != '') style.maxHeight = this.addUnit(minMaxHeight[1]);
			}
			return style;
		},
		// 中部弹窗的特有样式
		centerStyle() {
			let style = {};
			style.width = this.width ? this.addUnit(this.width) : 'fit-content';
			// 中部弹出的模式，如果没有设置高度，就用auto值，由内容撑开高度
			style.height = this.height ? this.addUnit(this.height) : 'fit-content';
			style.zIndex = this.uZindex;
			style.backgroundColor = this.popColor;
			style.marginTop = `-${this.addUnit(this.negativeTop)}`;
			if (this.borderRadius) {
				style.borderRadius = `${this.addUnit(this.borderRadius)}`;
				// 不加可能圆角无效
				style.overflow = 'hidden';
			}
			// 加入最小和最大高度或宽度
			let minMaxWidth = this.minMaxWidth.split(',');
			if (minMaxWidth.length == 1 && minMaxWidth[0] != '') style.minWidth = this.addUnit(minMaxWidth[0]);
			if (minMaxWidth.length == 2) {
				if (minMaxWidth[0] != '') style.minWidth = this.addUnit(minMaxWidth[0]);
				if (minMaxWidth[1] != '') style.maxWidth = this.addUnit(minMaxWidth[1]);
			}
			let minMaxHeight = this.minMaxHeight.split(',');
			if (minMaxHeight.length == 1 && minMaxHeight[0] != '') style.minHeight = this.addUnit(minMaxHeight[0]);
			if (minMaxHeight.length == 2) {
				if (minMaxHeight[0] != '') style.minHeight = this.addUnit(minMaxHeight[0]);
				if (minMaxHeight[1] != '') style.maxHeight = this.addUnit(minMaxHeight[1]);
			}
			// 容器内边距
			if (!this.menuable) style.padding = '0.1em 0.2em';
			else style.padding = '0 0.2em';
			if (!this.mask) style.boxShadow =this.boxShadow;
			return style;
		},		
		scrollStyle() {
			let style = {};
			//按官方教程中，竖向滚动时scroll-view需要设置固定高度，经测试设置最大高度也可，即内容超过这个最大高度会滚动
			//1、未设置高度和最大高度，即内容自适应高度，但要注意屏幕高度即是最大高度
			style.minWidth = '10vw';
			style.minHeight = '10vh';
			style.maxWidth = 'calc(100vw - 0.2em)';
			style.maxHeight = 'calc(100vh - 0.1em)';
			if (this.menuable) {
				//有菜单时处理
				style.maxHeight = 'calc(100vh - 2em)';
				if (this.mode.toLowerCase() == 'left' || this.mode.toLowerCase() == 'right') style.maxHeight = 'calc(100vh - 1em)';
			}
			//2、设置最大高度
			let minMaxWidth = this.minMaxWidth.split(',');
			if (minMaxWidth.length == 1 && minMaxWidth[0] != '') style.minWidth = 'calc(' + this.addUnit(minMaxWidth[0]) + ' - 0.2em)';
			if (minMaxWidth.length == 2) {
				if (minMaxWidth[0] != '') style.minWidth = 'calc(' + this.addUnit(minMaxWidth[0]) + ' - 0.2em)';
				if (minMaxWidth[1] != '') style.maxWidth = 'calc(' + this.addUnit(minMaxWidth[1]) + ' - 0.2em)';
			}
			let minMaxHeight = this.minMaxHeight.split(',');
			if (minMaxHeight.length == 1 && minMaxHeight[0] != '') style.minHeight = 'calc(' + this.addUnit(minMaxHeight[0]) + ' - 0.1em)';
			if (minMaxHeight.length == 2) {
				if (minMaxHeight[0] != '') style.minHeight = 'calc(' + this.addUnit(minMaxHeight[0]) + ' - 0.1em)';
				if (minMaxHeight[1] != '') {
					style.maxHeight = 'calc(' + this.addUnit(minMaxHeight[1]) + ' - 0.1em)';
					//有菜单时处理
					if (this.menuable) style.maxHeight = 'calc(' + this.addUnit(minMaxHeight[1]) + ' - 1em)';
				}
			}

			//3、设置高度
			if (this.height != '' && this.height != 'auto') {
				style.height = 'calc(' + this.addUnit(this.height) + ')';
				//居中弹窗的处理
				// if (this.mode.toLowerCase() == 'center') style.height = 'calc(' + this.addUnit(this.height) + ' - 5px)';
				//有菜单时处理
				if (this.menuable) style.height = 'calc(' + this.addUnit(this.height) + ' - 1em)';
			}

			return style;
		},
		maxStyle() {
			let style = {};
			style.width = '100vw';
			style.height = '100vh';
			style.zIndex = this.uZindex;
			style.backgroundColor = this.popColor;
			style.marginTop = `-${this.addUnit(this.negativeTop)}`;
			if (this.borderRadius) {
				style.borderRadius = `${this.addUnit(this.borderRadius)}`;
				// 不加可能圆角无效
				style.overflow = 'hidden';
			}
			return style;
		},
		maxScrollStyle() {
			let style=JSON.parse(JSON.stringify(this.maxStyle));
			if (this.menuable) style.height = 'calc(100vh - 1em)';
			return style;
		},
		// 计算整理后的z-index值
		uZindex() {
			return this.zIndex ? this.zIndex : 10075;
		},
		popMode() {
			// 过滤大小写
			return this.mode.toLowerCase();
		},
		fullscreenIconPath() {
			if (this.fullscreenIcon) {
				let fullscreenIcon = JSON.parse(JSON.stringify(this.fullscreenIcon));
				if (Object.prototype.toString.call(fullscreenIcon).slice(8, -1) === 'Array') {
					// 过滤不是字符串的图标名
					fullscreenIcon = fullscreenIcon.filter(item => Object.prototype.toString.call(item).slice(8, -1) === 'String');
					// 若是只有一个图标则复制一份
					if (fullscreenIcon.length == 1) fullscreenIcon.push(fullscreenIcon[0]);
					// 若是有2个成员则直接返回
					if (fullscreenIcon.length == 2) return fullscreenIcon;
					// 若是有2个以上成员则过滤掉
					if (fullscreenIcon.length > 2) return fullscreenIcon.filter((item, index) => index < 2);
				}
			}
			return ['fullscreen-line', 'fullscreen-exit-line'];
		}
	},
	watch: {
		value(val) {
			if (val) {
				this.open();
			} else if (!this.closeFromInner) {
				this.close();
			}
			this.closeFromInner = false;
		},
		maskCloseAble(val) {
			if (val) this.close();
		}
	},
	mounted() {
		//计算当前窗口可用宽高
		const res = uni.getSystemInfoSync();
		windowWidth = res.windowWidth;
		windowHeight = res.windowHeight;
		// 组件渲染完成时，检查value是否为true，如果是，弹出popup
		this.value && this.open();
		// 根据menuFull可设置显示即全屏
		if (this.fullScreen) this.fullscreen = true;
	},
	methods: {
		// 遮罩被点击
		maskClick(event) {
			//若遮罩被隐藏则无视关闭事件
			if (this.mask) this.close();
		},
		close() {
			// 标记关闭是内部发生的，否则修改了value值，导致watch中对value检测，导致再执行一遍close
			// 造成@close事件触发两次
			this.closeFromInner = true;
			this.change('showDrawer', 'visibleSync', false);
		},
		open() {
			//暂时不需要动态计算
			// this.$nextTick(() => {
			// 	if (this.mode.toLowerCase() == 'center') {
			// 		uni.createSelectorQuery()
			// 			.in(this)
			// 			.select('.u-mode-center-box')
			// 			.boundingClientRect(data => {
			// 				console.log('center', data);
			// 			})
			// 			.exec();
			// 	} else {
			// 		uni.createSelectorQuery()
			// 			.in(this)
			// 			.select('.u-drawer-content')
			// 			.boundingClientRect(data => {
			// 				console.log(this.mode.toLowerCase(), data);
			// 			})
			// 			.exec();
			// 	}
			// });
			this.change('visibleSync', 'showDrawer', true);
		},
		bindFullscreen() {
			this.$emit('fullscreen');
			this.fullscreen = true;
		},
		bindexitFullScreen() {
			this.$emit('exitfullscreen');
			this.fullscreen = false;
		},
		// 中部弹出时，需要.u-drawer-content将居中内容，此元素会铺满屏幕，点击需要关闭弹窗
		// 让其只在mode=center时起作用
		modeCenterClose(mode) {
			if (mode.toLowerCase() != 'center' || !this.maskCloseAble) return;
			this.close();
		},
		// 此处的原理是，关闭时先通过动画隐藏弹窗和遮罩，再移除整个组件
		// 打开时，先渲染组件，延时一定时间再让遮罩和弹窗的动画起作用
		change(param1, param2, status) {
			// 如果this.popup为false，意味着为picker，actionsheet等组件调用了popup组件
			if (this.popup == true) {
				this.$emit('input', status);
			}
			this[param1] = status;
			if (status) {
				// #ifdef H5 || MP
				this.timer = setTimeout(() => {
					this[param2] = status;
					this.$emit(status ? 'open' : 'close');
				}, 50);
				// #endif
				// #ifndef H5 || MP
				this.$nextTick(() => {
					this[param2] = status;
					this.$emit(status ? 'open' : 'close');
				});
				// #endif
			} else {
				this.timer = setTimeout(() => {
					this[param2] = status;
					this.$emit(status ? 'open' : 'close');
				}, this.duration);
			}
		},
		// 添加单位，如果数值则单位是rpx，其它如rpx，%，px，em,vw,vh等单位结尾或者值为auto，直接返回
		addUnit(value = 'auto', unit = 'rpx') {
			value = String(value);
			return /^(?:-?\d+|-?\d{1,3}(?:,\d{3})+)?(?:\.\d+)?$/.test(value) ? `${value}${unit}` : value;
		}
	}
};
</script>

<style lang="scss" scoped>
.u-drawer {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	overflow: hidden;
}

.u-drawer-content {
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	position: absolute;
	z-index: 1003;
	transition: all 0.25s linear;
	overflow: hidden;
}

.u-drawer__scroll-view {
	// width: 100%;//scroll-view已经内置width:100%
	// height: calc(100% - 10px); //动态计算高度
	//防止事件穿透
	pointer-events: auto;
}

.u-drawer-left {
	top: 0;
	bottom: 0;
	left: 0;
	margin: auto;
	background-color: #ffffff;
}

.u-drawer-right {
	right: 0;
	top: 0;
	bottom: 0;
	margin: auto;
	background-color: #ffffff;
}

.u-drawer-top {
	top: 0;
	left: 0;
	right: 0;
	margin: auto;
	background-color: #ffffff;
}

.u-drawer-bottom {
	bottom: 0;
	left: 0;
	right: 0;
	margin: auto;
	background-color: #ffffff;
}

.u-drawer-center {
	/* #ifndef APP-NVUE */
	display: flex;
	flex-direction: row;
	justify-content: center;
	align-items: center;
	/* #endif */
	flex-direction: column;
	bottom: 0;
	left: 0;
	right: 0;
	top: 0;
	margin: auto;
	opacity: 0;
	z-index: 9999;
}

.u-mode-center-box {
	min-width: 10vw;
	min-height: 10vh;
	max-width: 100vw;
	max-height: 100vh;
	/* #ifndef APP-NVUE */
	display: block;
	/* #endif */
	position: relative;
	background-color: #ffffff;
}

.u-drawer-content-visible.u-drawer-center {
	transform: scale(1);
	opacity: 1;
}

.u-animation-zoom {
	transform: scale(1.15);
}

.u-drawer-content-visible {
	transform: translate3D(0px, 0px, 0px) !important;
}

//菜单样式
.u-menu {
	border: 0;
	padding: 0;
	margin: 0 0.15em;
	height: 1em;
	display: flex;
	flex-flow: row nowrap;
	align-items: center;
	//防止事件穿透
	pointer-events: auto;
}

.u-menu--left {
	justify-content: flex-start;
}

.u-menu--right {
	justify-content: flex-end;
}

.u-menu--center {
	justify-content: center;
}
</style>
