---
title: Vue/cli3打包本地预览问题
date: '2020-06-10'
tag: 'Vue'
---

npm run build 默认生成了 dist 文件夹, 但是本地预览一片空白, 找不到资源;

官方提供的预览方案 :  [https://cli.vuejs.org/guide/deployment.html][1],

本地预览解决方案 : 

1. 修改 vue.config.js 下的 publicPath 配置为 './',

2. 此时可以本地预览,但是现实不完整, 打开路由 index.js, mode 改为 'hash'

3. 重新打包, 即可本地预览 dist 文件 index.html

  [1]: http://%20https://cli.vuejs.org/guide/deployment.html