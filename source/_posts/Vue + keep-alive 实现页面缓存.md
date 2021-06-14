---
title: Vue + Keep-alive实现返回上一级访问位置
date: '2020-10-26'
tag: 'Vue'
---

需求背景：用户访问网站路径为 [ 首页 → 商品详情页 → 首页 ]， 此时需要缓存首页，再次返回到首页，需要展示到上次浏览的锚点位置

开发环境：vue + vuex + router + keep-alive

### 解决方案：

**1：router + keep-alive**

缺陷：一旦缓存后，无法销毁，占用内存

```
大致步骤：
1 路由meta中添加一个标识：
  meta: { title: ''， isKeepAlive: true }
 
2 app.vue中使用keep-alive
 
  <keep-alive :max="10">
    <router-view v-if="$route.meta.isKeepAlive" />
  </keep-alive>
  <router-view v-if="!$route.meta.isKeepAlive" />
 
3 具体页面中通过 activated, deactivated钩子调用数据函数
```

**2：router + keep-alive + vuex (推荐)**
```
大致步骤：
 
1. vuex中设置需要缓存的组件名： keepAlivePage: []
 
 
2. app.vue
<keep-alive :include="keeplist" :max="10">
  <router-view></router-view>
</keep-alive>
 
<script>
  import { mapGetters } from 'vuex'
  export default {
    computed: {
      ...mapGetters({
        keeplist: 'getKeepAlivePage'
      })
    }
  }
</script>
 
 
3. store -> getters.js
const getters = {
  getKeepAlivePage: state => {
    return state.keepAlivePage
  }
}
export default getters
 
 
4. store -> mutation.js
 
const mutations = {
  // 缓存某个组件
  cache (state, name) {
    if (!state.keepAlivePage.includes(name)) {
      state.keepAlivePage.push(name)
    }
  },
 
  // 不缓存某个组件
  uncache (state, name) {
    if (state.keepAlivePage.includes(name)) {
      state.keepAlivePage = state.keepAlivePage.filter(item => item !== name)
    }
  }
}
 
 
5. router.js
const routeArry = [] // 路由
const router = new VueRouter({
  mode: 'history',
  routes: routeArry,
  // vue-router提供了scrollBehavior方法
  scrollBehavior (to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { x: 0, y: 0 }
    }
  }
})
 
 
6. 准备工作完毕，接下来愉快的使用啦，以开头需求背景为例：home.vue
import { mapMutations } from 'vuex'
<script>
  created () {
    this.setKeepAlive('Home')
  },
  
  methods: {
   ...mapMutations({
     setKeepAlive: 'cache',
     delKeepAlive: 'uncache'
   })
  },
 
  beforeRouteLeave (to, from, next) {
    if (to.name !== 'goodsdetail') {
      this.delKeepAlive('Home')
    }
    next()
  }
</script>
```
 
**划重点：delKeepAlive('Home') 和 setKeepAlive('Home') 中，传值一定是组件名称， 非路由name**

 

### 大致思路：

进入页面，先将组件缓存下来；

通过beforeRouteLeave判断，是否为需求指定页面，如果是跳过，反之清除掉；

这样就可以及时销毁缓存啦~