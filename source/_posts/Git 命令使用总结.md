---
title: Git 常用命令总结
date: '2017-06-19 22:15'
tag: ['Git', 'Linux']
---

Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目；Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件；Git与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。


---


### 常用命令
```javascript
git init // 初始化文件

git config -list // 查看仓库配置

// 关联远程仓库
git config --global user.name "" 
git config --global user.email ""

// 创建,查看SSH-key
创建： ssh-keygen -t rsa -C 123456@qq.com
查看： cd ~/.ssh
      cat id_rsa.pub

// 克隆远程仓库
git clone git@github.com/xxxxx/xxx.git

// 本地与Github关联
git remote add origin git@github.com/xxxxx/xxx.git

// 查看改动状态
// -s表示short，输出两列标记，第一列staging区域,第二列working目录
git status -s

// 1. 创建本地分支，创建一个dev的本地开发分支
git checkout -b dev

// 查看分支
git branch -a 

// 切换分支
git checkout <your branch name>

// 合并分支，先切换到主分支master，然后合并开发分支dev
git merge dev

// 拉取远程分支master / dev
git push origin master 或 git push -u origin master

// 查看推送状态
git remote -v 

// 查看日志，方便回退
git log 或者 git relog 

// 回退版本
git reset --hard ^ 或者 git reset --hard xxxxx版本号

// 提交改动文件
git add . 或者 git add xxx.html

// 提交改动备注
git commit -m "xxxx修改" 

// 拉取远程分支代码
git pull origin <your branch name>

// 代码提交远程分支
git push origin <your branch name>

// 上传文件到服务器
scp index.zip root@17.52.255.3:/usr/share/nginx/html
```

- - -

### 项目分支管理

背景：项目由多人共同参与开发时

开发：克隆远程仓库到本地，新建本地开发分支，开发完成后合并到master，然后推送到远程

**a). 常用基本流程**
```javascript
git clone https://github.com/xxxx/xxx.git   克隆远程仓库到本地
git checkout -b dev   新建本地开发分支
git status   查看改动文件
git add .   提交全部文件
git commit -m""   添加改动标记
git branch   查看当前分支
git checkout master   切换到主分支master
git pull origin master   拉取远程主分支代码
git merge dev   合并本地开发分支到master
git push origin master   推送本地到远程master
git checkout dev   切换到本地开发分支

=========== 如果粗心忘记先拉去后合并，且还没有提交 ===========

第一种方式：查看提交记录，回退到之前版本：
git log ;
git reset --hard xxxxx

第二种方式：将远程拉取到本地，使用git stash pop
git stash   将本地代码stash到仓库
git pull   拉取远程代码
git stash pop   将仓库代码合并到本地最新代码
git add .
git commit -m""
git pull origin master
git push origin master
```

**b). 仓库多分支独立**
```
git clone https://github.com/xxx/xxx.git
git branch -a
git checkout --orphan <branch name> // 创建与主干不同步的分支
git checkout <branch name>
git rm -rf . // 如果引用了主干，先删除内容，添加自己的内容
git add .
git commit -m 'xxx'
git push origin <branch name>
```