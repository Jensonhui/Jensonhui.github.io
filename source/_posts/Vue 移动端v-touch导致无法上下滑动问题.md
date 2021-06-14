---
title: v-touch 导致移动端无法上下滑动
date: '2019-10-24'
tag: ['Vue', 'Css']
---

### 一、[v-touch，vue组件问题]

vue 引用组件v-touch 导致html无法上下滚动
当引用v-touch时, html标签默认会添加一个 <kbd>touch-action: none;</kbd> 属性

> touch-action定义:
用于指定某个给定的区域是否允许用户操作，以及如何响应用户操作（比如浏览器自带的划动、缩放等）

> touch-action属性(IE10之前不支持):
1 . auto  默认值, 浏览器允许一些手势（touch）操作在设置了此属性的元素上, 例如: 对视口（viewport）平移/缩放等操作
2 . none  禁止触发默认的手势操作
3 . pan-x  可以在父级元素(the nearest ancestor)内进行水平移动的手势操作
4 . pan-y  可以在父级元素内进行垂直移动的手势操作
5 . manipulation  允许手势水平/垂直平移或持续的缩放, 任何auto属性支持的额外操作都不支持

#### 解决方案, 直接style样式覆盖:
```css
touch-action: pan-y!important;
```

- - -

### 二、[-webkit-overflow-scrolling 滑动不流畅问题]

-webkit-overflow-scrolling 用来控制元素在移动设备上是否使用滚动回弹效果

```css
body {
     overflow:auto;   /* 用于 android4+，或其他设备 */
     -webkit-overflow-scrolling:touch;    /* 用于 ios5+ */  
}

// 当手指从触摸屏上移开，滚动会立即停止
-webkit-overflow-scrolling: auto;  

// 当手指从触摸屏上移开，会保持一段时间的滚动
webkit-overflow-scrolling: touch; 

```

