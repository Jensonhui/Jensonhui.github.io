---
title: Vue 路由守卫验证登录状态
date: '2020-07-02'
tag: 'Vue'
---

router.js
```
  // 登录标识，needlogin
  {
    path: '/goodsblock',
    name: 'goodsblock',
    component: () => import('@/views/goods/GoodsBlock.vue'),
    meta: { title: '好物', endMenuShow: true, needlogin: true }
  }
```
&nbsp;

router - index.js
```
router.beforeEach((to, from, next) => {
  // 404
  if (to.matched.length === 0) { next('/404') }

  // 改变 title
  if (to.meta.title) { document.title = to.meta.title }

  // 验证登录状态, 注意死循环，判断路由=login进行next()
  const ttoken = cookie.getCookie('logins')
  if (to.matched.some(record => record.meta.needlogin)) {
    if (ttoken && ttoken.token) { next(); return } else {
      next({ name: 'login', query: { redirect: to.fullPath } })
      return
    }
  }
  next()
})
```
&nbsp;

login.vue
```
  // 登录返回200，存token及用户信息后，跳转至携带参数地址
  if (code === 200) {
    localStorage.setItem("ttoken", data.token);
    this.$router.push({ path: decodeURIComponent(this.$route.query.redirect) || '/' })
  }
```
&nbsp;

mine.vue //我的需要登录
```
  this.$router.push({
    path: '/login',
    // 携带当前页面的路由，这样登录会回到当前页
    query: { redirect: '/mine' } 
  })
```


