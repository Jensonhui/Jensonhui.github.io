---
title: Git一个仓库，不同branch管理不同项目
date: '2021/02/02 13:48:00'
tag: 'Git'
---

项目多了，Git需要建立很多仓库管理，太繁琐了。

于是乎想用同一个仓库，不同分支管理项目，但新建分支默认继承父亲tree，而我们需要的两个没有关联的分支

传送门  = [git-checkout (ctrl + f 搜索orphan)][1]

  [1]: https://git-scm.com/docs/git-checkout


### 教程开始：

1 . 新建一个仓库操作（避免失误把之前的仓库搞坏了）, 可以用命令或者在GitHub控制台操作



2 . 新建后默认分配了master主分支，将仓库下载到本地：
```
git clone https://github.com/xxx/xxx<仓库名称>.git

```


3 . 下载到本地，用终端打开，执行以下命令：

```
git branch // 会展示master
 
git status // 查看状态，是否有需要提交
```
此时，将你想放到master的项目copy到仓库文件夹下，再执行
```
git add .
git commit -m 'master'
git push origin master
 
// 此时已经完成master主分支项目提交了
```


4 .  以上三步完成了第一个分支推送，接下来创建第二个分支
```
git checkout --orphan xxx<new branch name>
 
// 创建一个与主分支无关联的新分支
 
```
创建完毕，如果默认继承了master tree，这时需要清除索引和工作树，继续执行：
```
git rm -rf . // 注意后面的点
```
执行完毕，将新分支的项目copy到仓库文件夹下，执行推送命令即可，注意后面更换分支名称
```
这里假设新分支名称为：next-branch
 
git add .
git commit -m 'next-branch commit'
git push origin next-branch
 
// 此时已经完成新分支next-branch的推送
```


