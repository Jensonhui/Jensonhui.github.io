---
title: Vue 实现底部固定 tab 导航
date: '2020-06-15'
tag: ['Vue', 'Css']
---

**方法一：通过$route.path判断跳转**
```javascript
<template>
  <ul class='end-menu flex-row'>
    <li
      v-for='(item, index) in menulist'
      :key='index'
      @click='changeMenu(item.path)'
      :class="[$route.path === item.path ? 'active' : '']"
    >
      <img :src='$route.path === item.path ? item.active : item.normal' alt=''>
      <span>{{item.title}}</span>
    </li>
  </ul>
</template>


<script>
export default {
  name: 'endMenu',
  data () {
    return {
      // 直接粘贴路由表过来
      menulist: [
        {
          path: '/home',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '首页'
        },
        {
          path: '/living',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '直播'
        },
        {
          path: '/goodsblock',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '好物'
        },
        {
          path: '/carlist',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '购物车'
        },
        {
          path: '/mine',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '我的'
        }
      ]
    }
  },
  methods: {
    changeMenu (path) {
      if (this.$route.path === path) { return }
      this.$router.replace(path)
    }
  }
}
</script>
```
&nbsp;

**方法二：通过router-link跳转**
```javascript
<template>
  <div class='end-menu flex-row'>
    <router-link
      v-for='(item, index) in menulist'
      :key='index'
      :to='item.path'
      active-class='active'
    >
      <img :src='active === index ? item.active : item.normal' alt=''>
      <span>{{item.title}}</span>
    </router-link>
  </div>
</template>


<script>
export default {
  name: 'endMenu',
  data () {
    return {
      menulist: [
        {
          path: '/home',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '首页'
        },
        {
          path: '/living',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '直播'
        },
        {
          path: '/goodsblock',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '好物'
        },
        {
          path: '/carlist',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '购物车'
        },
        {
          path: '/mine',
          normal: require('@/assets/img/logo.png'),
          active: require('@/assets/img/logo.png'),
          title: '我的'
        }
      ]
    }
  }
}
</script>

```

