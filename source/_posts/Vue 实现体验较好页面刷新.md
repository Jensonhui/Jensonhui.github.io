---
title: Vue/provide/inject实现页面刷新
date: '2020-01-02'
tag: 'Vue'
---

传统方法会使页面强制刷新出现空白，体验不好
```Javascript
location.reload();
this.$rotuer.go(0);
```

原理: 通过v-if来渲染<router-view>

provide：选项应该是在一个对象或者返回一个对象的函数。该对象包含可注入其子孙的属性
inject：一个字符串数组，或者一个对象，对象的key是本地的绑定名

----------

App.vue添加provide属性
```Javascript
<template>
  <div id="app">
    <router-view v-if="isRouterAlive"/>
  </div>
</template>

<script>
export default {
  name: 'app',
  provide () {
    return {
      pageReload: this.pageReload
    }
  },
  data () {
    return {
      isRouterAlive: true
    }
  },
  methods: {
    pageReload () {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
      })
    }
  }
}
</script>

```
<br/>

子组件inject引用父组件的属性
```Javascript
export default {
  name: 'childTemplate',
  inject: ['pageReload'],
  methods: {
    reloadFun () {
      this.pageReload() // 调用
    }
  }
}
```