---
title: Vue动态改变url参数无法获取新数据问题
date: '2020-10-26'
tag: 'Vue'
---

**需求背景：**
Vue通过url参数获取当前页数据，动态改变query参数后，返回无法获取最新数据问题

A页面通过query: { sno: xxxx } 跳转到B页面

B页面通过接收sno参数，请求获取数据：

```javascript
// 查询商品详情, 在created生命周期调用
async getGoodsInfo () {
  const res = await this.$post(this.$url.goods.gooddetail, {
      sno: this.$route.query.sno
  })
}
```

B页面底部有相关产品列表，点击产品动态改变当前query中的sno值：
```javascript
// 点击相关热销商品
changeDetailSno (skuSno) {
  const query = this.$router.history.current.query
  const path = this.$router.history.current.path
  const newQuery = JSON.parse(JSON.stringify(query))
  newQuery.id = val;
  this.$router.push({ path, query: newQuery })
 
  window.scrollTo(0, 0)
  this.getGoodsInfo()
}
```
 


**问题描述：**动态改变query参数后，返回上一级后，发现数据还是最后一次请求的数据，没有更新为对应id的数据

 

**解决方案：**通过监听路由，重新调用获取list方法
```javascript
watch: {
  // 监听路由，调用详情方法
  $route: 'getGoodsInfo'
}
```