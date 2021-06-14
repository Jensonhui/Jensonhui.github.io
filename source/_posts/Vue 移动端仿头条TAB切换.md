---
title: Vue 移动端仿头条TAB切换
date: '2019-10-19 09:10'
tag: ['Vue', 'Css']
---

效果跟头条的Tab跟随差不多,暂时不能实现tab始终固定在中间显示 !

没有找到合适的插件, 采用了VUE的 Transition + v-touch 以及 Animate.css 手写实现 !

[传送门-Github =>][1]

**使用注意:** 

移动端滑动, v-touch 需要进行脚手架依赖安装才可以使用

1 . cnpm install vue-touch@next --save dev

2 . main.js 引入使用  import VueTouch from 'vue-touch' ; Vue.use(VueTouch, { name: 'v-touch' })

**类似的UI库**

[vux.li][2]

[Swiper.js][3]

[Muse-ui][4]


  [1]: https://github.com/Jensonhui/vue-toutiao-tab
  [2]: https://vux.li/demos/v2/?x-page=v2-doc-home#/component/tab
  [3]: https://www.swiper.com.cn/demo/index.html
  [4]: https://muse-ui.org/#/zh-CN
