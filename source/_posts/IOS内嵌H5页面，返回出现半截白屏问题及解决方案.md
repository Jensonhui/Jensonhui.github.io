---
title: IOS内嵌H5页面，半截白屏问题及解决方案
date: '2021/03/25 10:48:32'
---

## 场景 ##
【ios内嵌加载h5页面】A页面加载数据 ---跳转B页面后返回  --- 返回A页面会显示一部分白屏，随意滑动页面后，页面显示完整（android下没事）

## 分析 ##
可能跟ios回弹效果，组件有关系，[iOS WebView加载网页触摸白屏bug排查；][1]    [附上VUE-ISSUES][2]


## 解决方案 ##

**方案一：（使用中， 简单粗暴）**
```javascript
History.scrollRestoration
 
定义：允许web应用程序在历史导航上显式地设置默认滚动恢复行为
 
参数值：
1. auto 将恢复用户已滚动到的页面上的位置
2. manual 未还原页上的位置。用户必须手动滚动到该位置
 
 
// 通过路由守卫，判断设置manual
router.beforeEach((to, from, next) ={
  if (history.scrollRestoration) {
    history.scrollRestoration = 'manual'
  }
})

```

**方案二**：（有效果但不适用于滚动加载）
```javascript
在数据请求接口完成后，添加如下代码
 
// 模拟回到顶部
this.$nextTick(() ={
  window.scrollTo(0, 1); 
  window.scrollTo(0, 0);
})
 
 
// 举例
async getDeductionList () {
  try {
    const res = await this.post(url, {page: 1})
    const { code, list } = res.data
    if (code === 200') {
      // 非滚动加载
      this.deductionList = list
      this.$nextTick(() ={
        window.scrollTo(0, 1);
        window.scrollTo(0, 0);
      })
      // 滚动加载
      this.page++
      this.deductionList = [...this.deductionList, ...list]
      if (this.page === 1) {
        this.$nextTick(() ={
          window.scrollTo(0, 1);
          window.scrollTo(0, 0);
        })
      }
    }
  } catch (error) {
    console.log(error)
  }
}
// 滚动加载
scrollLoad () {
  this.getDeductionList()
}
```

**方案三**：（测试未起效果）
```css
html, body {
  position: relative;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
 
#app {
  position: absolute;
  left:0;
  top:0;
  width: 100%;
  height: 100%;
  background: #fff;
  overflow: scroll;
  -webkit-overflow-scrolling: touch;
}
 
#app * {
  transform: translateZ(0px)
  -webkit-transform: translateZ(0px)
}
 
```


  [1]: http://www.poorren.com/ios-webview-white-screen-bug-fixes
  [2]: https://github.com/vuejs/vue/issues/5533