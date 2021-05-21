# 初始命令（查看、配置）
1. git 测试是否安装成功
2. git --version  查看git版本
3. git config --global user.name "用户名"
4. git config --global user.email "用户邮箱"
5. git config --global commit.template.git_template 提交模板设置


# 添加、提交、查看
1. 版本库、仓库 repository  git init
2. git add 文件 将文件添加到仓库
3. git commit -m "本次提交说明" 将文件提交到仓库
4. git status 查看结果，时刻掌握仓库当前状态
5. git diff 文件   查看文件difference 修改



# 版本回退
1. git log  查看历史记录，显示从最近到最远的提交日志
2. git log --pretty=oneline  精简历史信息
3. 当前版本 HEAD，上一个版本 HEAD^，上上个版本 HEAD^^,前100个版本 HEAD~100
4. git reset --hard HEAD^   回退上一个版本
5. git reset --hard commit id   回退到某个版本
6. git reflog 记录每一次命令


# 工作区和暂存区
+ 工作区（work directory）：在电脑里能看到的目录

+ 版本库（repository）：工作区中隐藏目录.git，即Git的版本库

+ 暂存区（stage，index）：在Git的版本库中

+ Git自动创建的第一个master分支，指向master的一个指针HEAD
  ![add和commit过程](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620456902103_2.png)

  

git add 将文件修改从工作区添到暂存区
git commit 将暂存区中所有内容提交到当前分支
需要提交的⽂文件修改通通放到暂存区，然后一次性提交暂存区的所有修改。

一旦提交后，对工作区没有任何修改，那么工作区就是“干净”的


# 管理修改
Git 跟踪并管理的是修改，而非文件
每次修改，如果不add到暂存区，那就不会加入到commit中。
- git diff HEAD -- 文件名
- 查看工作区和版本库里面最新版本的区别

# 撤销修改
- git checkout -- 文件名    将文件在工作区的修改全部撤销，分两种情况
1. 文件修改后还未放入暂存区，现在，撤销修改就回到和版本库一模一样的状态
2. 文件已经添加到暂存区后，又做了修改，现在撤销修改就回到添加到暂存区后的状态
总之，让该文件回到最近一次git add或git commit时的状态

- git reset HEAD <file> 将暂存区的修改撤销掉（unstage），重新放回工作区

git reset   回退版本、将暂存区的修改回退到工作区

- 当你工作区中的内容改错了，想直接丢弃工作区的修改：git checkout -- file
- 当你不但改乱了工作区中某个文件的内容，还添加到了暂存区：git reset HEAD file，然后 git checkout -- file
- 已经提交到了版本库，撤销本次提交，版本回退
git reset --hard 版本id

# 删除文件
在Git中，删除也是一种修改操作，在工作区中删除了某个文件，这个时候，工作区和版本库不一致，两种情况：
1. 删除错误，从版本库中回复到工作区：git checkout -- file。git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以一键还原
2. 将版本库更新：git rm file，git commit -m "本次提交说明"


# 远程仓库
1. 创建SSH key：ssh-keygen -t rsa -C "email@example.com"
2. 在github或gitlab中添加ssh公钥
3. 添加远程库，在github或gitlab上新建仓库
4. 在本地的仓库下运行 git remote add origin git@github.com:github账户名/远程仓库名，将本地仓库和远程仓库关联
5. 本地库推送至远程库：git push -u orign master 实际上是将当前分支master推送至远程。由于远程是空的，第一次推送master分支时，参数-u，git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在之后的推送和拉取时就可以简化命令。
6. 推送：git push origin master

# 从远程库克隆
1. git clone git@github.com:github账户名/远程仓库.git
Git支持多种协议，ssh，https等


# 分支管理
## 创建与合并分支
一开始，只有master一个分支，master指向提交，而HEAD指向master。
每次提交，master向前移动一步，随着不断提交，master分支的线也就越来越长。

当创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，表示当前分支在dev上。

![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620462396882_2.png)
![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620462415708_3.png)

从现在开始，对工作区的修改和提交针对dev分支，新提交一次后，dev指针往前移动一步，而master指针不变。

合并：直接把master的指针指向dev的当前提交，就完成了合并
![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620465983343_2.png)
![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620466012242_3.png)
合并完分支之后，可以删除dev分支，删除dev分支就是把dev指针删除


+ 创建分支：git checkout -b dev -b 参数代表创建并切换
+ git branch dev
+ git checkout dev

+ 查看当前分支：git branch
+ 合并分支：git merge dev   将dev分支成果合并到master分支上，git merge用于将指定分支合并到当前分支


+ 删除分支：git branch -d dev

**总结**
- 查看分支：git branch
- 创建分支：git branch name
- 切换分支：git checkout name
- 创建+切换分支：git checkout -b name
- 合并某分支到当前分支：git merge name
- 删除分支：git branch -d name


# 分支管理策略
通常，合并分支时，如果可能，Git会用“Fast forward”模式，但是这种模式下，删除分支后，会丢掉分支信息。
如果强制禁用“Fast forward” 模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就能看出分支信息。

+ git merge --no-ff -m "merge with on-ff" dev
禁用“Fast forward”


# Bug 
每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
- git stash 把当前工作现场“储藏”起来，等以后恢复现场后继续工作
- 确定在哪个分支进行修复，进行切换，创建分支，比如在master分支上修复
- git checkout master
- git checkout -b issue-101
- 修复完成后，切换回原分支，并进行合并，最后删除issue-101分支
- 切换原工作分支dev，git checkout dev，
- git stash list，查看stash的工作现场，
- 恢复工作现场，两种方法：
1. git stash pop，恢复的同时把stash内容也删了
2. git stash apply,不删除stash内容
3. 需要使用 git stash drop 删除

# 解决冲突
![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620610310305_3.png)
```
# 创建并切换分支
git checkout -b feature1
# 在readme.txt 文件中添加最后一行
# 添加并提交
git add redme.txt
git commit

#切换master分支，修改readme。添加、提交，产生冲突，无法使用fast faward

```

![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620610310305_3.png)

必须手动解决冲突后再提交

Git⽤用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

![](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620613366188_6.png)

```
# 带参数的git log 也可以查看分支合并情况
git log --graph --pretty=oneline --abbrev-commit
```

# Feature 分支
添加新功能，新建feature 分支，开发完成后进行合并，最后删除该feature 分支

git branch -D name 
强行删除分支

# 多人协作
当从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，远程仓库的名字默认为origin


```
# 查看远程仓库信息
git remote origin
git remote -v
origin git@github.com:michaelliao/learngit.git(fetch)
origin git@github.com:michaelliao/learngit.git(push)

# 推送分支
git push origin master

git push origin dev
```

并不是所有本地分支都一定要往远程推送
+ master 是主分支，因此时刻需要与远程同步；
+ dev分支是开发分支，团队所有成员都需要在上面工作，也需要远程同步
+ bug分支只用于在本地修复bug
+ feature 分支是否推送到远程，取决于是否合作开发

## 抓取分支
git clone git@github.com:michaellion/learngit.git

默认情况，从远程库clone时，只能看到本地master分支
因此创建远程其他分支到本地：
git checkout -b dev origin/dev

![1](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620614338714_7.png)

![2](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620614393921_8.png)


# git 标签
![3](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620614495294_9.png)

![4](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620614520256_10.png)


![5](C:\Users\admin\Desktop\笔记\DailyNotes\20210521\img\1620614599681_11.png)