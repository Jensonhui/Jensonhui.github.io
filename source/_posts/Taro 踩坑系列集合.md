---
title: ReactNative 之 Taro 踩坑系列集合
date: '2019-07-14 15:35'
tag: 'Taro'
---

### 一、'isWatch' undefined

错误描述：TypeError: Cannot read property 'isWatch' of undefined

操作过程：
```
删除项目下 node_modules

重新 npm install

执行 npm run dev:h5
```

解决方案：
```
taro update self
taro update project

相当于更新节点 和 npm
```

[---------原文链接---------][1]
  [1]: https://github.com/NervJS/taro/issues/3321


----------


### 二、H5代理配置

问题描述：公众号开发，在公众号开发工具中，请求接口时报错，配置下代理即可解决

配置过程：
```
    ======== config下配置devServer ========
h5: {
  publicPath: '/',
  staticDirectory: 'static',
  devServer: {
    port: 8868,
    proxy: {
      '/upload': {
        target: "'https://xx.xxx.cc",
        pathRewrite: {
          "/upload": ""
        },
        changeOrigin: true
      }
    }
  }
}

```
[---------原文链接---------][2]
  [2]: https://github.com/NervJS/taro/issues/4001

----------


### 三、自定义组件
自定义组件时候，引入报错，model not found： 

解决方案：自定义组件名称必须是**大写**开头，否则Taro识别不到是组件



