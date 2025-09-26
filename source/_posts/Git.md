---
title: Git
date: 2024-01-27 12:54:45
categories: 
- 工具
tags:
- 版本管理
---

[Git 中文文档](https://git-scm.com/book/zh/v2) 阅读笔记

> 有空多看看这个 [Git 笔记汇总](https://github.com/xirong/my-git) 

# Chapter 1 起步
## 版本控制

1.  集中化版本控制（CVCS）缺点 
   1. 中央服务器的单点故障就无法协同工作
   2. 本地只有快照，项目整体和其变更历史只保存在服务器上
2.  分布式版本控制（DVCS）优点 
   1. 代码仓库完整地镜像下来，包括完整的历史记录
   2. 用户在本地保存了所有改动，每一次的克隆都是对仓库的完整备份
   3. 可以指定和若干不同的远端代码仓库进行交互
## Git 简史

1. 并行开发
2. 完全分布式
3. 速度快
4. 数据量大
## Git 简介

1. 大多版本控制工具都是基于差异的，存储文件随时间变化的差异，而Git储存项目快照或索引
2. Git 安全性优，只做增不做删；完整性好，使用SHA-1 哈希算法
3. 文件的四种状态：已提交（committed）、已修改（modified）、已暂存（staged）、未跟踪（untracked）
4. 项目的三个阶段：工作目录、暂存区域以及 Git 仓库
5. 工作流程：修改文件、暂存文件、将暂存区文件快照永久存在Git目录
## Git 配置
> git config --list --show-origin

1. /etc/gitconfig 系统通用配置 git config --system
2. ~/.gitconfig 当前用户配置 git config --global
3. .git/config 当前项目配置 git config --local
# Chapter 2 基础
> Git命令只能控制当前文件夹和子文件夹内的修改

## 基础命令

1. git init 
   1. 初始化仓库
2. git clone 
   1. 克隆仓库
3. git status 
   1. 查看文件状态
   2. [-s] 简要文件状态信息
4. git add 
   1. [] 跟踪新文件
   2. [] 已修改的文件放到暂存区
   3. [] 合并时把有冲突的文件标记为已解决状态
5. git diff 
   1. 已修改文件与已暂存文件的内容差异
   2. [--staged] 已暂存与分支HEAD的内容差异
6. git rm 
   1. [] 记录此次移除文件的操作
   2. [-f] 强制移除存在未暂存修改的文件
   3. [ --cached] 文件保留在工作区中，从Git仓库和暂存区中删除
7. git commit 
   1. 提交文件
   2. [-m] 提交备注
   3. [--amend] 追加提交
   4. [-a] 直接将工作区和暂存区的所有修改一起提交
8. git mv 
   1. [ ] 移动文件/改名
9. git log 
   1. 回顾提交历史
   2. [--stat] 显示每次提交的简要信息
   3. [-p ] 显示每次提交的详细信息
   4. [--pretty=format:"xxx"] 自定义信息格式
   5. [--oneline --graph --all] 显示提交历史、各个分支的指向以及项目的分支分叉情况
   6. [--since] 限制时间
10. git reset 
   1. [HEAD ] 取消暂存
11. git checkout 
   1. [-- ] 撤销修改
   2. [/]  切换分支
   3. [-b  ] 创建本地和远程分支并切换，本地与远程分支相关联
12. git remote 
   1. [-v] 列出远程仓库
   2. [add  ] 添加远程仓库
   3. [remane  ] 修改仓库本地名称
   4. [show ] 查看远程仓库信息
   5. [remove ] 移除远程仓库
13. git fetch 
   1. [] 从远程仓库抓取
14. git push 
   1. [  ] 推送到远程分支
   2. [ /--tags] 推送标签/所有标签
   3. [ --delete ] 删除远程分支
   4. [-u  --all]  本地的仓库和远程仓库进行关联
   5. [--force] 强制推送
15. git pull 
   1. [  ] 拉取到本地分支并合并
16. git tag 
   1. 	列出已有的标签
   2. [-l <通配符>] 查询某些对应标签
   3. [] 轻量标签（提交校验和存储到一个文件中）
   4. [-a  -m ] 创建附注标签（数据库中的一个完整对象， 可以被校验）
   5. [-d] 删除标签
17. git show 
   1. 标签信息和与之对应的提交信息
18. git restore 
   1. [] 丢弃工作区修改
   2. [--staged ] 暂存区修改放回工作区
19. git merge 
   1. [] 合并当前分支到branch分支
20. git branch 
   1. [-d ] 删除分支
   2. [-v] 每个分支的最后一次提交
   3. [-vv] 跟踪信息
   4. (--set-upstream-to= | -u ) [] 跟踪上游分支
   5. [--merged --no-merged] 合并/未合并到当前分支的其他分支
21. git stash 
   1. 贮藏工作
   2. [list] 列出所有贮藏
   3. [apply] 恢复贮藏
22. git rebase 
   1. [ ] 变基
## 忽略文件
.gitignore 正则表达式：[官方样例文件](https://github.com/github/gitignore)
# Chapter 3 分支
> 相关命令合并到 Chapter2 中

1. Git 中 HEAD是一个指针，指向当前工作位置
2. git mergetool 图形化冲突解决工具
3. 有 Git 特性演化出的开发工作流：长期分支、主题分支
## 变基
如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。只对尚未推送或分享给别人的本地修改执行变基操作清理历史， 从不对已推送至别处的提交执行变基操作。

工作原理：提取内的修改并存为临时文件，然后将HEAD指向，最后以此将之前另存为临时文件的修改依序应用
# Chapter 6 GitHub
