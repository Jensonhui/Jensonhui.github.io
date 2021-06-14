---
title: Marked-markdown 使用采坑
date: '2020-08-06'
tag: 'Vue'
---
安装：
npm install marked  or  cnpm install marked

css:
npm install github-markdown-css  or  cnpm install github-markdown-css


使用:

```
<template>
  <!-- 注意：需要添加 markdown-body class类名 -->
  <div class="item-cont markdown-body" id="markDownHtml"></div>
</template>

<script>
import marked from 'marked'
import 'github-markdown-css'
export default {
  mounted () {
    const htmls = '| test  |  test |\n' +
      '| ------------ | ------------ |\n' +
      '|   test|  test |\n' +
      '![](http://10.8.9.206:8080/15973220503558878.png)'
    document.getElementById('markDownHtml').innerHTML = marked(htmls)
  }
}
</script>

```


----------

插件官方地址：

1 [Typora-markdown.css][1]
2 [Marked Documentation][2]
3 [github-markdown-css][3]


  [1]: http://theme.typora.io/
  [2]: https://marked.js.org/
  [3]: https://github.com/sindresorhus/github-markdown-css