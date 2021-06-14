---
title: Safari 定位fixed下，z-index层级失效问题
date: '2018-06-29 10:24'
tag: ''
---

1 . Safari font-size的px单位和vm单位计算不支持，需要是百分比单位
2 . Safari 渐变，从#fff到transparent会有灰色带，其他浏览器都是白色到透明
3 . Safiari 伪元素hover时候的currentColor不渲染....

<br/>

### Safari下Position或3D变换时，z-index层级失效

在Safari浏览器下，此Safari浏览器包括iOS的Safari，iPhone上的微信浏览器，以及Mac OS X系统的Safari浏览器，当我们使用3D transform变换的时候，如果祖先元素没有overflow:hidden/scroll/auto等限制，则会直接忽略自身和其他元素的z-index层叠顺序设置，而直接使用真实世界的3D视角进行渲染。

<br/>

### z-index层级失效 解决方案

> 【方案1】父级，任意父级，非body级别，设置overflow:hidden可恢复和其他浏览器一样的渲染。

> 【方案2】以毒攻毒，有时候，页面复杂，我们不能给父级设置overflow:hidden，直接给fixed容器添加transform: translateZ(100px);


<br/>

### 传送门(原著链接)
https://www.zhangxinxu.com/wordpress/2016/08/safari-3d-transform-z-index/
