---
title: Vue在history模式下调用微信分享
date: '2020-03-20'
tag: ['Vue', 'Javascript']
---

History服务端配置忽略; 以下为vue前端部分;
场景: 单页面详情页需要配置分享, 解决微信二次分享图片,链接丢失问题

思路如下: 
1 . 由于苹果系统的特殊性, 需要单独设置页面的URL, 所以在路由跳出时判断路径

2 . 安卓直接location.href, 苹果系统单独给window下添加一个变量 "entryUrl" 保存

3 . 导出一个公众的wechatAuth方法, 用于调用微信的配置方法 

## 第一步 ##
安装微信JSDK
```
npm insatll -g weixin-js-sdk
// 安装完查看package.json, 是否有weixin-js-sdk版本信息
// ["weixin-js-sdk": "^1.4.0-test"]
```

## 第二步 ##
在Router配置文件下:
```
import Vue from 'vue'
import Router from 'vue-router'
import { wechatAuth } from '@/common/js/weXinShare'  // 分享JS

new Router({
  mode: 'history',
  base: '/app-duiyu',
  routes: [
    {
      path: '/shareGoodsDetail', // 商品分享
      name: 'shareGoodsDetail',
      component: resolve => require(['@/pages/shareGoodsDetail.vue'], resolve),
      meta: {
        title: '商品分享',
        allowShare: true
      }
    },
})

// 微信配置
router.afterEach((to, from) => {
  let allowShare = Boolean(to.meta.allowShare)
  let authUrl = window.location.href.split('#')[0]
  if (window.__wxjs_is_wkwebview) {
    if (window.entryUrl === '' || window.entryUrl === undefined) {
      window.entryUrl = authUrl
    }
    setTimeout(() => { wechatAuth(authUrl, 'ios', allowShare) }, 1000)
  } else {
    setTimeout(() => { wechatAuth(authUrl, 'android', allowShare) }, 1000)
  }
})
```

## 第三步 ##
进入详情时候通过VUEX进行数据保存

导出"wechatAuth"方法
```
import axios from 'axios'
import wx from 'weixin-js-sdk'
import state from '@/store/state'

export const wechatAuth = async (authUrl, device, allowShare) => {
  if (!allowShare) { return false }
  try {
    let formalURL = '正式开发接口地址, 拿到公众号配置信息'
    let localURL = device === 'ios' ? window.entryUrl : authUrl

    let authRes = await axios.get(formalURL, {params: { url: encodeURIComponent(localURL) }})
    const {code, object: configInfo, message} = authRes.data

    if (code === 200 && !!state.shareGoods.name) {
      let shareLise = state.shareGoods
      let shareData = {
        title: shareLise.name, // 分享标题
        desc: shareLise.title, // 分享描述
        link: localURL, // 分享连接
        imgUrl: shareLise.masterImgUrl // 分享图标
      }
      wx.config({
        debug: false,
        appId: configInfo.appId, // 必填，公众号的唯一标识
        timestamp: configInfo.timestamp, // 必填，生成签名的时间戳
        nonceStr: configInfo.nonceStr, // 必填，生成签名的随机串
        signature: configInfo.signature, // 必填，签名
        jsApiList: [ 'checkJsApi', 'updateAppMessageShareData', 'updateTimelineShareData', 'onMenuShareAppMessage', 'onMenuShareTimeline' ]
      })

      wx.checkJsApi({
        jsApiList: [
          'updateAppMessageShareData',
          'updateTimelineShareData',
          'onMenuShareAppMessage',
          'onMenuShareTimeline'
        ]
      })

      wx.ready(() => {
        wx.updateAppMessageShareData(shareData)
        wx.updateTimelineShareData(shareData)
      })
    } else {
      console.log(message)
      console.log('shareGoods is not defined')
    }
  } catch (e) {
    console.log(e)
  }
}
```
至此, 大功告成.


----------
## 踩坑区 ##
1 . 报错invalid url domain, 检查公众号是否配置域名正确, (区分域名和项目路径)

2 . 报错invalid signature, 大部分是路径的问题, 检查下传值路径是否和配置的域名对应

3 . 最好是配置绑定顶级域名(3次机会/月), 这样即使使用二级域名也可以使用

4 . 新版貌似调用分享API, 还需要把旧版的方法加上即"jsApiList(↑ ↑)", 不然调用会有问题 !

参考链接:
[WX-JSSDK][1]
[网页授权证书配置][2]
[安卓和苹果获取URL不同处理][3]
[配置了安全域名依然报invalid url domain][4]
[Vue项目history模式下微信分享总结][5]


  [1]: https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html
  [2]: https://blog.csdn.net/Garlic246/article/details/83304488?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
  [3]: https://www.cnblogs.com/blessYou/p/11491827.html
  [4]: https://segmentfault.com/q/1010000002516668
  [5]: https://segmentfault.com/a/1190000018689334