---
title: Javascript 判断浏览器H5跳转
date: '2018-04-27 17:32'
tag: 'Javascript'
---

```javascript
var mobile_bs = {
  versions: function() {
    var u = navigator.userAgent;
    return {
      trident: u.indexOf('Trident') > -1, //IE内核
      presto: u.indexOf('Presto') > -1,  //opera内核
      webKit: u.indexOf('AppleWebKit') > -1,  //苹果、谷歌内核
      gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1,  //火狐内核
      mobile: !! u.match(/AppleWebKit.*Mobile.*/) || !! u.match(/AppleWebKit/) && u.indexOf('QIHU') && u.indexOf('QIHU') > -1 && u.indexOf('Chrome') < 0,  //是否为移动终端
      ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),  //ios终端
      android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1,  //android终端或者uc浏览器
      iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1,   //是否为iPhone或者QQHD浏览器
      iPad: u.indexOf('iPad') > -1,     //是否iPad
      webApp: u.indexOf('Safari') == -1   //是否web应该程序，没有头部与底部
    }
  } ()
};

if (mobile_bs.versions.mobile) {
    if (mobile_bs.versions.android || mobile_bs.versions.iPhone || mobile_bs.versions.iPad || mobile_bs.versions.ios) {
        window.location.href = "./h5.html";
    }
};

```