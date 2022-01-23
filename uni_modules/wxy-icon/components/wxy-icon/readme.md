#wxy-icon图标集-最优质且统一风格的图标库，源于remixicon

- [访问remixicon官网](https://remixicon.com/)
- [特点](## 特点)
- [使用说明](## 使用说明)

## 特点
- remixicon提倡全部开源，免费商用。
- 每一个图标都由remixicon成员绘制，设计统一。
- CDN引入，无需引入繁琐的字体等文件。
- 支持NVue, Vue, 小程序，H5

## 使用说明
安装之后会在components目录下多一个wxy-uniapp的文件夹，文件夹结构如下:

```
-wyx-icon
 - icon.js
 - wxy-icon.vue
 - readme.md
```

组件提供如下props参数:

| props | 类型 | 说明 |
| :-----| ----: | :----: |
| name | String | 图标名,官方命名规则:ri-类名-line/fill(线状/填充),在官方图标上点击即可查看更多细节,本组件引用js已经移除ri前缀，其它一样 |
| color | String | 颜色填充,支持rgb()、十六进制或关键字 |
| size | Number,String | 大小,增加支持的字符串:%|px|rpx|em|vw|vh|vmax|vmin|auto，默认单位是rpx，默认大小是20rpx |
| click | 事件 | 点击 Icon 触发事件 |

示例:

```html
<wxy-icon name="add-fill" color="white" size="2em" @click="btnMenu"></wxy-icon>
```

图标类名请在[remixicon中搜索](https://remixicon.com/)

提示: 不要在name中写入图标库的前缀ri，组件中已经处理好前缀，不用担心类名冲突的问题