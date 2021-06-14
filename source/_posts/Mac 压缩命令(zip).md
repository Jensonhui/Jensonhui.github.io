---
title: Mac压缩命令(zip)
date: '2020-12-16'
tag: [Mac, Linux]
---

## 压缩文件 ##
1 . cd 目录

2 . 执行 zip -q -r app-agent.zip app-agent
```
-q	表示不显示压缩进度状态

-r	表示子目录子文件全部压缩为zip；这部分比较重要，不然的话只有something这个文件夹被压缩，里面的没有被压缩进去

-e	表示你的压缩文件需要加密，终端会提示你输入密码的；还有种加密方法，这种是直接在命令行里做的，比如zip -r -P Password01! modudu.zip SomeDir, 就直接用Password01!来加密modudu.zip了

-m	表示压缩完删除原文件

-o	表示设置所有被压缩文件的最后修改时间为当前压缩时间
```


## 解压文件 ##
1 . cd 目录

2 . 执行 unzip -l 文件名(app-agent.zip)