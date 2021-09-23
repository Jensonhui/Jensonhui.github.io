---
title: CentOs7.x + 宝塔Linux + Typecho 路径404问题
date: '2021-09-03 09:59'
tag: ['CentOs', 'Linux', 'Web']
---

typecho搭建成功后，导入备份的数据SQL，列表内容访问会报404, 初步确认为Nginx配置的问题，

度娘了一大波，终于找到眉目了，宝塔默认会`开启伪静态`，这跟typecho永久链接设置冲突，

**解决方案：** `网站-设置-伪静态-选择typecho-保存`