# wxy-icon 介绍

基于插件市场[i-icon](https://ext.dcloud.net.cn/plugin?id=4042) 增强改进而来，这里首先感谢原作者。同时我对官方uni-icons也进行了增强，参考地址[https://ext.dcloud.net.cn/plugin?id=6716](https://ext.dcloud.net.cn/plugin?id=6716)
二者使用时最大区别就是图标名，uni-icons是type，uni-icon是name，其它没什么区别

[访问remixicon官网图标演示](https://remixicon.com/)

## 组件改进

1. 支持设置图标margin，默认是0
2. 支持点击、长按、触屏事件
3. 同uni-icons一样支持扩展图标
4. 支持是否冒泡，并解决了小程序端阻止冒泡时不触发点击的问题

## 平台兼容性

同[i-icon](https://ext.dcloud.net.cn/plugin?id=4042)兼容性

## 参数说明
|属性名		|类型|默认值	|说明|
|:-:		|:-:		|:-:		|:-:|
|name		|String	|-			|图标名,官方命名规则:ri-类名-line/fill(线状/填充),本组件引用js已经移除ri前缀,	|
|size		|Number\|String	|1em	|大小尺寸，增加支持的字符串:%\|px\|rpx\|em\|vw\|vh\|vmax\|vmin\|auto|
|color		|String	|black|颜色类型，支持rgb()、十六进制或关键字|
|customPrefix|String|-	|自定义图标|
|bold   	|Number\|String|noraml	|字体粗细|
|margin|String|0|外边距，margin规范,增加支持的字符串%\|px\|rpx\|em\|vw\|vh\|vmax\|vmin\|auto，默认单位是px，默认大小是0|
|bubble|Boolean|false|是否冒泡，默认是不冒泡|

		
|事件称名|说明|返回值|
|:-:|:-:|:-:|
|click|点击 Icon 触发事件|event|
|longpress|长按 Icon 触发事件|event|
|touchstart	|触摸Icon开始触发事件|event	|
|touchmove	|触摸Icon移动触发事件|event	|
|touchend	|触摸Icon结束触发事件|event	|

## 基本使用

### 简单使用

```vue
<wxy-icon name="lock-line"></wxy-icon>
```

### 事件冒泡

```vue
<wxy-icon name="lock-line" bubble></wxy-icon>
```

### 设置大小、边距，支持带单位

```vue
<wxy-icon name="contact" size="2vw" margin="5px 10px"></wxy-icon>
```

### 扩展图标

```vue
<wxy-icon name="biaoqing" custom-prefix="chat"></wxy-icon>
```