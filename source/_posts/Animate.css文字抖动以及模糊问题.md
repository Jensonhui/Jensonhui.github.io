---
title: Animate.css文字抖动以及模糊问题
date: '2019-10-19 18:06'
tag: 'Css'
---

传送门 => [Just-add-water CSS animations - Animate.css][1]

```
 1 . 在该动画的transform里加上translateZ(0)值,能解决文字抖动的问题，但是没解决文字模糊的问题。

 2 . 在发生文字模糊的地方加上transform: translate3d(0,0,0)，解决文字模糊以及的问题。

 使用scale发生文字抖动及模糊时，在该文字的css中只需要加入transform: translate3d(0,0,0)即可解决

```

  [1]: https://daneden.github.io/animate.css/