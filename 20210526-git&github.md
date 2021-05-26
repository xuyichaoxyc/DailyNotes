[TOC]

# 工作原理流程

|                |				|                    |							pull							|                    |

|                |				|                    |							checkout							|workspace |
	   ———fetch/clone———>
| Remote |				|Repository |				|                    |				|                    |

|                |				|                    |				|      index    |				|                    |
	   <———    push      ———
|                |				|                    |		commit		|                    |		add		|                    |



Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库



**workspace** ——（add）——> **index** ——（commit）——> **Repository**

**Repository** ——（checkout）——> **workspace**

**Repository** ——（push）——> **Remote**

**Remote** ——（fetch/clone）——> **Repository**

**Repository** ——（pull）——> **Workspace**

# 本地操作

## 本地提交操作

### git init

### git add file

### git commit -m "description"

### git checkout -- file	撤销，丢弃工作区的修改	/ git restore

1. 未add到暂存区，回退到版本库一样的状态
2. 已添加到暂存区，回退到添加暂存区后的状态

## 本地查询操作

### git status

### git diff file

### git log/ git log --pretty=oneline

### git reflog

## 分支操作

### git branch

### git branch branchname

### git checkout branchname

### git checkout -b branchname

### git merge branchname

### git branch -d name

## 版本回退

### git reset --hard HEAD^/HEAD^^/HEAD^^^..../HEAD~100

### git reset --hard banbenhao 



## 暂存操作

### git stash

### git stash list

### git stash apply

### git stash drop

### git stash pop



# 远程操作

## git remote add origin https://github.com/username/xx.git

## git remote add origin git@github.com:username/xx.git

## git remote -v

## git push -u origin branchname

## git push origin --delete branchname

## git clone https://github.com/username/xx.git

## git clone git@github.com:username/xx.git

## git branch -a

## git fetch

## git checkout -b dev origin/dev

## git branch --set-upstream dev origin/dev

## git pull

### 拉取远程分支

4、把远程分支拉到本地

**git fetch origin dev（dev为远程仓库的分支名）**

5、在本地创建分支dev并切换到该分支

**git checkout -b dev(本地分支名称) origin/dev(远程分支名称)**

6、把某个分支上的内容都拉取到本地

**git pull origin dev(远程分支名称)**

