# wxy-popup 介绍

**重要提醒:** 组件属性部分有所改变,若是使用0.0.3以前版本，属性名以下面说明为准

## 组件来源

wxy-popup是基于**uViewUI的popup**增强改进而来，这里要感谢vViewUI，主要是以下几个方面：

1. 尺寸单位支持更多，如数值时单位是rpx，也支持带单位rpx,px,em，vw,vh，%和auto等
2. 弹出层宽度和高度，支持用户自定义了，默认宽高是fit-content,即自适应内容
3. 最小最大宽度和高度，中心弹出层时默认最小宽度10vw，最小高度是10vh，支持用户设定最小最大宽度或高度
4. 采用更合理的布局实现了限宽和限高情况下居中的实现
5. 借鉴layer优点，上下左右支持关闭功能，中心弹窗支持关闭以及全屏/恢复功能
6. 上下左右弹窗时可指定距离边缘的距离，应用场景更灵活了，如弹窗中放个音乐播放，这样就可以是悬浮式音乐播放了，其它可以自由发挥了
7. 组件已经在H5、APP和微信小程序上通过测试，可适应横竖屏，其它可自行测试，也可修改源码
8. 在隐藏遮罩层情况下，支持事件穿透，即可点击弹窗以外区域事件

## 组件优势

- 相比于uViewUI更强调用户体验和用户自定义性，不再"僵硬"式显示
- 相比于layer的mobile版，更强调原生组件的使用，纯js虽好，但无生态
- 目前0.0.4版已经能满足各种弹窗需求了,唯一不足就是APP对原生组件的处理，以后有时间再完善了

## 注意事项

为了正确使用弹窗容器，这里要重点说明使用注意事项，这里可区分所有内容包在一个容器中或直接在弹窗容器中布局所有两种情况，上面案例中都是包在一个容器中再放到弹窗容器

- 包在一个容器中时

如案例中`<view class="pop-container">...</view>`,此时pop-container要设置width:100%,高度可不设置，但 **千万不要设置overflow为hidden** 若是设置hidden则自己处理滚动逻辑

- 全部放在弹窗容器

每一行宽度建议设置为100%，让它自适应弹窗窗器宽度，如果要自己设置宽度，则要加上max-width:100%限制下，不然可能宽度超过弹窗超过宽度



## 示例代码

### 基本案例：
```html
...
<wxy-popup v-model="show" :mode="mode" :height="height" :width="width" :border-radius="radius">
	<view class="pop-container">
		<view class="pop-item title center"><text>弹窗示例</text></view>
		<view class="pop-item subtitle"><text>主要完成弹窗容器，可提供弹窗相关操作</text></view>
		<view class="pop-item"><text>1、提供上、下、左、右和中五个位置弹窗</text></view>
		<view class="pop-item"><text>2、提供了默认设置，可简化调用</text></view>
		<view class="pop-item"><text>3、可设置宽度或高度，若超过高度则自动滚动</text></view>
		<view class="pop-item"><text>4、可设置圆角、宽度、高度、最大(小)宽度或高度</text></view>
	</view>
</wxy-popup>
...
<script>
export default {
	data() {
		return {
			show: false,
			mode: 'center',
			height: '',
			width: '',
			radius: '14',
		};
	},
};
</script>
```

### 最大最小高度示例：
```vue
<wxy-popup v-model="show" :mode="mode" min-max-height="30vh,50vh">
	<view class="pop-container">
		<view class="pop-item title center"><text>弹窗示例</text></view>
		<view class="pop-item subtitle"><text>主要完成弹窗容器，可提供弹窗相关操作</text></view>
		<view class="pop-item"><text>1、提供上、下、左、右和中五个位置弹窗</text></view>
		<view class="pop-item"><text>2、提供了默认设置，可简化调用</text></view>
		<view class="pop-item"><text>3、可设置宽度或高度，若超过高度则自动滚动</text></view>
		<view class="pop-item"><text>4、可设置圆角、宽度、高度、最大(小)宽度或高度</text></view>
	</view>
</wxy-popup>
```

### 边缘距离示例：
```vue
<wxy-popup v-model="show" :mode="bottom" distane="10%">
<wxy-audio src="https://jiustech.ygsjyxx.cn/uploads/school/202107/60fa690877b52.mp3" :play.sync="audioPlay"></wxy-audio>
</wxy-popup>
```

## 参数说明
* @property {String}  mode  弹出方向（默认left）
* @property {Boolean} mask  是否显示遮罩（默认true）
* @property {Numberr | String} border-radius 弹窗圆角值（默认0）
* @property {Boolean} mask-close-able        点击遮罩是否可以关闭弹出层（默认true）,，前提是mask是true
*********
* @property {Boolean} menuable         是否显示菜单图标（默认false）
* @property {Boolean} menuFull  全屏和恢复图标是否显示,默认是false,它作用前提是menuable是true
* @property {String}  menu-position    菜单图标位置,可选left,center,right，默认是right;
* @property {Boolean} fullScreen       是否全屏,默认是false
* @property {String}  close-icon       关闭图标的名称，指定名称由https://remixicon.com/中而来
* @property {String}  close-icon-color 关闭图标的颜色（默认#909399）
* @property {Array}   fullscreen-icon  全屏和恢复图标的名称，指定名称由https://remixicon.com/中而来，要注意是成对设置,用逗号分隔
* @property {String}  fullscreen-icon-color 全屏和恢复图标的颜色（默认#909399）
*********
* @property {Object} custom-style 用户自定义样式
* @property {Stringr | Number} negative-top 中部弹出时，往上偏移的值
* @property {Numberr | String} z-index 弹出内容的z-index值（默认1075）
* @property {Boolean} zoom  是否开启缩放动画，只在mode为center时有效（默认true）
* @property {Boolean} safe-area-inset-bottom 是否开启底部安全区适配（默认false）
*********
* @event {Function} open 弹出层打开
* @event {Function} close 弹出层收起
*********
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
* @property {String} box-shadow        遮罩层隐藏状态下弹窗阴影，默认是'0 0 8px 4px #333333'