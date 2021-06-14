---
title: H5如何与Android和IOS通信
date: '2019-10-24'
tag: 'Vue'
---

### 交互方式

Android: 
> 1) window.WebViewJavascriptBridge[methodName](params)
  2) window.android.methodName

IOS: 
> 1) window.webkit.messageHandlers[methodName].postMessage(params) // 无法获取原生返回值
  2) window.prompt(message, default) // 可以获取到返回值


-----------

```javascript
// 判断设备
export const getAppCode = () => {
  const ua = navigator.userAgent.toLowerCase()
  if (/(Android)/i.test(ua)) { return 'android' }
  if (/(iPhone|iPad|iPod|iOS)/i.test(ua)) { return 'ios' }
}

```

<br/>

```javascript
// 封装使用
export const implement = (funcName, isCallBack = true, params) => {
  try {
    // android
    if (getAppCode() === 'android') {
      if (isCallBack) {
        if (params) {
          return window.android[funcName](params)
        } else {
          return window.android[funcName]()
        }
      } else {
        if (params) {
          window.android[funcName](params)
        } else {
          window.android[funcName]()
        }
      }
    }


    // ios
    if (getAppCode() === 'ios') {
      if (isCallBack) {
        if (params) {
          return window.prompt(JSON.stringify({ type: 'JSbridge', functionName: funcName, arguments: { params } }))
        } else {
          return window.prompt(JSON.stringify({ type: 'JSbridge', functionName: funcName, arguments: {} }))
        }
      } else {
        if (params) {
          window.webkit.messageHandlers[funcName].postMessage(params)
        } else {
          window.webkit.messageHandlers[funcName].postMessage()
        }
      }
    }
  } catch (error) {
    console.log(error)
  }
}
```