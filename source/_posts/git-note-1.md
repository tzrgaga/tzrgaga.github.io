---
layout: post
title:  "git使用笔记"
date:   2016-03-11
categories: git
excerpt: git
---


最近在看关于git方面的博客文章和书籍，在这里顺便做些笔记，正所谓温故而知新嘛！

## **Git基本原理**


#### **1.直接记录快照，而非差异比较**

git和其他版本控制系统的主要差别在于，git只关心文件数据的整体是否发生变化，而大多数其他版本控制系统只关心文件内容的具体差异。
git更像是把变化的文件做快照，记录在一个微型的文件系统中。每次提交更新时，扫描一遍所有文件的指纹信息(`SHA-1哈希值`)并对文件做一快照，让后保存指向这次快照的索引。若文件没有变化，git不会再次保存，而只对上次保存的快照做一链接。

工作方式如图所示：

![git工作方式](https://git-scm.com/figures/18333fig0105-tn.png)


#### **2. 近乎所有操作都是本地执行**

在git中的绝大多数操作都只需要访问本地文件和资源，不需要连接网络。

#### **3. 时刻保持数据完整性**

在保存到git之前，所有的数据都要进行内容的校验和(`checksum`)计算，并将此结果作为数据的唯一标识和索引。
git使用SHA-1算法计算数据的校验和，通过对文件的内容和目录的结构计算出一个`SHA-1哈希值`，作为指纹字符串。
该字符串由`40个十六进制字符`(0-9及a~f)组成。

#### **4. 多数操作仅添加数据**

常用的git操作大多仅仅是把数据添加到数据库(`.git目录`)。

#### **5. 文件的三种状态**

对于任何一个文件，在git中都只有3种状态：

1. **已提交(committed)** 表示该文件已被安全地保存在本地数据库，即执行了`git commit`命令
2. **已修改(modified)** 表示修改了某个文件，但是还没有提交保存
3. **已暂存(staged)** 表示把已修改的文件放在下次提交时要保存的清单中，也就是执行了`git add`命令

git管理项目时，文件流转的3个工作区域：

示意图：

![git管理项目时，文件流转的3个工作区域](http://git-scm.com/figures/18333fig0106-tn.png)

1. **工作目录** git对某个目录进行版本控制，那么这个目录就叫做工作目录
2. **暂存区域** 一般存放在`.git`目录中
3. **本地仓库** 一般就是执行了`git init`所在的目录

基本的git工作流程如下：

> 1. 在工作目录修改某些文件
> 2. 对修改的文件进行快照，然后保存在暂存区域
> 3. 提交更新，讲保存在暂存区域的文件快照永久转储到`.git`目录中

## **常用命令**

| 命令        | 说明           |
| ------------- |:-------------|
| `git init`      | 初始化项目 |
| `git status`      | 查看项目状态      |
| `git add <. or 文件名>` | 添加到追踪列表(暂存区域)      |
| `git commit <-m "提交说明">` | 将改动正式提交      |
| `git log` | 查看项目历史记录      |
| `git clone` | 克隆(下载)项目      |
| `git branch <分支名>` | 创建分支    |
|` git checkout <分支名 or -- xx文件名> `| 分支切换和恢复文件      |
| `git merge <分支名>` | 合并分支(先切换到master分支)      |
| `git tag <-a 标签名 -m "说明信息">` | 为版本打标签      |
| `git help` | 帮助文档      |
| `git remote add origin` | 与远程项目关联      |
| `git remote -v` | 查看该项目的远程仓库地址      |
| `git push` | 推送项目到远程仓库      |
| `git pull` | 拉取远程仓库最新代码      |

## **Github协作流程**

> 1. Fork主仓库得到派生项目
> 2. 通过 `git clone`或者`git pull`将派生项目的代码拉取到本地
> 3. 本地修改代码实现功能
> 4. 通过`git add`与`git commit`命令提交代码到本地
> 5. 通过`git pull`命令从主仓库对应的分支更新代码，如有冲突则解决冲突
> 6. 将本地代码提交到派生项目对应的分支中
> 7. 向主仓库发送Pull request请求
> 8. 主仓库管理员合并Pull request
> 9. 完成一轮协作







