[TOC]

### 一、Git

分布式版本控制系统

工作区、暂存区、版本库、中心服务器(远程仓库)



### 二、Git相关操作

#### 1. 创建仓库（git init）

**git init**：初始化一个Git仓库，生成一个.git目录，该目录包含了资源的所有元数据

**git init newrepo**：指定目录作为Git仓库

#### 2. 克隆仓库（git clone）

**git clone**：从现有Git 仓库中拷贝项目

```
git clone <repo>

// 克隆到指定目录
git clone <repo> <directory>
```

#### 3. 配置（git config）

显示当前的git 配置信息：git config --list

编辑git配置文件：git config -e（针对当前仓库）

​							 git config -e --global（针对系统上所有的仓库）

设置提交代码时的用户信息：

git config --global user.name "runoob"

git config --global user.email test@runoob.com



#### 4. 提交与修改（git add、git commit、git reset、git reset、git rm、git mv、git status、git diff）

| 命令         | 说明                                     |
| :----------- | :--------------------------------------- |
| `git add`    | 添加文件到暂存区                         |
| `git status` | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`   | 比较文件的不同，即暂存区和工作区的差异。 |
| `git commit` | 提交暂存区到本地仓库。                   |
| `git reset`  | 回退版本。                               |
| `git rm`     | 删除工作区文件。                         |
| `git mv`     | 移动或重命名工作区文件。                 |

#### 5. 提交日志

| 命令               | 说明                                 |
| :----------------- | :----------------------------------- |
| `git log`          | 查看历史提交记录                     |
| `git blame <file>` | 以列表形式查看指定文件的历史修改记录 |

#### 6. 远程操作

| 命令         | 说明               |
| :----------- | :----------------- |
| `git remote` | 远程仓库操作       |
| `git fetch`  | 从远程获取代码库   |
| `git pull`   | 下载远程代码并合并 |
| `git push`   | 上传远程代码并合并 |



### 三、分支管理

#### 1. 创建分支命令（git branch (branchname)）

#### 2. 切换分支命令（git checkout (branchname)）

#### 3. 合并分支命令（git merge）



#### 4. 列出分支（git branch）

#### 5. 创建新分支并切换（git checkout -b (branchname)）

#### 6. 删除分支（git branch -d (branchname)）



### 四、查看提交历史

git log 

git blame <file>





### 五、远程仓库相关操作（Github）

#### 1. 添加远程仓库（git remote add [shortname] [url]）

#### 2. 查看当前的远程库（git remote）

#### 3. 提取远程仓库（git fetch，git merge）