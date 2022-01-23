<template>
	<text
		v-if="bubble"
		class="i-icon"
		:class="[customPrefix, customPrefix ? name : '']"
		:style="[iconStyle]"
		@click="_click"
		@longpress="_longpress"
		@touchstart="_start"
		@touchend="_end"
		@touchmove="_move"
	>
		{{ iconname }}
	</text>
	<text
		v-else
		class="i-icon"
		:class="[customPrefix, customPrefix ? name : '']"
		:style="[iconStyle]"
		@click.stop="_click"
		@longpress.stop="_longpress"
		@touchstart="_start"
		@touchend="_end"
		@touchmove.stop="_move"
	>
		{{ iconname }}
	</text>
</template>
<script>
/**
 * 基于插件市场https://ext.dcloud.net.cn/plugin?id=4042改进(与wxy-icons不同是，wxy-icons是基于官方uni-icons图标库增强)
 * @tutorial https://remixicon.com/
 * @property {String} name	图标名,官方命名规则:ri-类名-line/fill(线状/填充),在官方图标上点击即可查看更多细节,本组件引用js已经移除ri前缀，其它一样
 * @property {String} color 颜色填充,支持rgb()、十六进制或关键字
 * @property {Number|String} size 大小，增加支持的字符串:%|px|rpx|em|vw|vh|vmax|vmin|auto，默认是1em
 * @property {Number|String} bold  字体粗细,默认是normal
 * @property {String} margin  外边距，margin规范,增加支持的字符串%|px|rpx|em|vw|vh|vmax|vmin|auto，默认单位是px，默认大小是0
 * @property {Boolean} bubble  是否冒泡，默认是false
 * @property {String} customPrefix 自定义图标
 * @event {Function} click 点击 Icon 触发事件
 * @event {Function} longpress 长按 Icon 触发事件
 *
 * 增加图标可参考官方https://www.uviewui.com/guide/customIcon.html
 * 扩展知识:
 * click和touch事件执行顺序：touchstart --> touchmove -> touchend -> click
 * 阻止冒泡对事件影响：APP端只影响父级，而微信小程序若是阻止touchstart或touchend冒泡则自动阻止click事件
 * 目前方案：当阻止冒泡时，通过touch的move判断是否触发click事件
 * 阻止冒泡：若是移动端或支持触屏时阻止冒泡，要注意touch与click触发顺序，若是阻止点击可模拟触发；最简单阻止某事件冒泡就是加上stop修辞符,功能为空,如@click.stop=""
 */
import icon from './icon.js';
// #ifdef APP-NVUE
var domModule = weex.requireModule('dom');
import iconUrl from './remixicon.ttf';
// 这里的CDN和css中的CDN一致
domModule.addRule('fontFace', {
	fontFamily: 'remixicon',
	src: "url('https://cdn.jsdelivr.net/npm/remixicon@2.5.0/fonts/remixicon.ttf?t=1590207869815')"
	// src: "url('" + iconUrl + "')"
});
// #endif

//以下函数来自于uView-UI2.0版，在此感谢
// 添加单位，如果有rpx，upx，%，px,em,vw,vh等单位结尾或者值为auto，直接返回，否则加上px单位结尾
function _unit(value = 'auto', unit = 'px') {
	value = String(value);
	// 用uView内置验证规则中的number判断是否为数值
	if (value == 0) return value; //0不用再加单位
	return /^[\+-]?(\d+\.?\d*|\.\d+|\d\.\d+e\+\d+)$/.test(value) ? `${value}${unit}` : value;
}
//判断是否为空
function _empty(value) {
	switch (typeof value) {
		case 'undefined':
			return true;
		case 'string':
			if (value.replace(/(^[ \t\n\r]*)|([ \t\n\r]*$)/g, '').length == 0) return true;
			break;
		case 'boolean':
			if (!value) return true;
			break;
		case 'number':
			if (value === 0 || isNaN(value)) return true;
			break;
		case 'object':
			if (value === null || value.length === 0) return true;
			for (const i in value) {
				return false;
			}
			return true;
	}
	return false;
}
export default {
	name: 'WxyIcon',
	emits: ['click', 'longpress', 'touchstart', 'touchmove', 'touchend'],
	props: {
		name: {
			type: String,
			default: ''
		},
		color: {
			type: String,
			default: ''
		},
		size: {
			type: [Number, String],
			default: '1em'
		},
		customPrefix: {
			type: String,
			default: ''
		},
		// 字体粗细
		bold: {
			type: [Number, String],
			default: 'normal'
		},
		margin: {
			type: String,
			default: '0'
		},
		// 是否冒泡
		bubble: {
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			icon,
			wxclick: false //解决微信小程序阻止冒泡时无法响应click事件
		};
	},
	computed: {
		iconname() {
			const { icon, customPrefix, name } = this;
			if (customPrefix == '') return icon[name];
			return '';
		},
		iconStyle() {
			let style = {};
			if (!_empty(this.color)) style.color = this.color;
			if (!_empty(this.bold)) style.fontWeight = this.bold;
			if (!_empty(this.size)) style.fontSize = _unit(this.size);
			if (!_empty(this.margin) && this.margin != 0) {
				const margin = String(this.margin);
				const marginArr = margin.split(' ').map(item => _unit(item));
				style.margin = marginArr.join(' ');
			}
			return style;
		}
	},
	methods: {
		_click(e) {
			// #ifdef MP
			//防止触发两次click事件
			if (!this.wxclick) this.$emit('click', e);
			// #endif
			// #ifndef MP
			this.$emit('click', e);
			// #endif
		},
		_longpress(e) {
			this.$emit('longpress', e);
		},
		_start(e) {
			this.wxclick = true;
			this.startData = {};
			// this.startData.clientX = e.changedTouches[0].clientX; //手指按下时的X坐标
			// this.startData.clientY = e.changedTouches[0].clientY; //手指按下时的Y坐标
			this.startData.clientX = e.touches[0].clientX; //手指按下时的X坐标
			this.startData.clientY = e.touches[0].clientY; //手指按下时的Y坐标
			this.$emit('touchstart', e);
		},
		_move(e) {
			let touch = e.touches[0]; //滑动过程中，手指滑动的坐标信息 返回的是Objcet对象
			if (Math.abs(touch.clientX - startData.clientX) > 50 || Math.abs(touch.clientY - startData.clientY) > 50) this.wxclick = false;
			this.$emit('touchmove', e);
		},
		_end(e) {
			this.$emit('touchend', e);
			// #ifdef MP
			if (this.wxclick) this.$emit('click', e);
			// #endif
		}
	}
};
</script>
<style lang="scss">
/* #ifndef APP-NVUE */
@font-face {
	font-family: remixicon;
	src: url('https://cdn.jsdelivr.net/npm/remixicon@2.5.0/fonts/remixicon.ttf?t=1590207869815') format('truetype');
	// src: url('./remixicon.ttf') format('truetype');
}
/* #endif */
.i-icon {
	/* 内级块 */
	display: inline-flex;
	font-family: remixicon;
	text-decoration: none;
	font-style: normal;
	/* 水平垂直居中 */
	justify-content: center;
	align-items: center;
}
</style>
