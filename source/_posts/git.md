---
title: git命令总结
date: 2019-05-18 12:11:12
categories: "Git" #文章分類目錄 可以省略
tags: [Git]
---
#Git介绍
***
定义：Linus Torvalds 开发的一个开放源码的版本控制软件，是一个开源的分布式版本控制系统、内容管理系统(CMS)，工作管理系统等（记录你的文本文件每次修改，协作团队编辑等）
对手：SVN、CVS等
<!--more-->


## 登录
当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中
在Git Bash中输入：
```
#当前项目注册，只有当前项目使用这个账号
git config user.name "Your Name"
git config user.email "email@example.com"
#全局注册，这台机器上所有的Git仓库都会使用这个账号
git config --global user.name "Your Name"   
git config --global user.email "email@example.com"
```
查看你的配置：
```
git config --list   #列出所有 Git 当时能找到的配置
git config user.name   #检查 Git 的某一项配置
```
查看帮助手册
```
git help <verb>
```
# 本地仓库
## 创建Git库
版本库又名仓库，英文名repository，版本库中所有文件都可以被Git管理、追踪、记录。
创建一个空的版本库：
```
git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，不可删除，默认是隐藏文件夹，可以设置文件夹查看文件的设置来查看.git文件，也可以运行命令`ls -ah`来查看当前目录的隐藏文件

## Git提交本地库流程
```
git add .   #添加全部文件到仓库
#git add <filename>
git add <文件.后缀>  #添加特定文件到仓库
git add <文件.后缀> <文件.后缀>  #添加多个特定文件到仓库
git commit -m "comment message"    #提交并添加本次提交的说明，每一次提交都是一个仓库快照，可以用于恢复或回退
```
git add命令可以执行多次。
# 状态管理
## 查看仓库状态
```
#查看仓库当前的状态，可以知道修改的文件，当前提交的状态等
git status           #查看仓库当前的状态
git status –short  #查看仓库当前状态的简化
git status -s  #git status –short简写
git diff <文件名.后缀>   #查看特定文件修改对比，diff就是difference,只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动
git diff HEAD -- <文件名.后缀>  #查看工作区与已提交的最新版本的特定文件的区别  
```

![image.png](https://upload-images.jianshu.io/upload_images/15009210-eff0fcc5678d9fcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/15009210-2f934a32eb53ff4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 查看历史记录
```
git log    #显示从最近到最远的提交日志
git log  --pretty=oneline   #简化日志，把每次提交展现为一行
git log -p -2 # -p，用来显示每次提交的内容差异。  -2 仅显示最近两次提交
git log --stat    #提交的简略的统计信息
git log --pretty=format:"%h - %an, %ar : %s"  # format，可以定制要显示的记录格式。
git log --since=2.weeks    #列出所有最近两周内的提交
git reflog     #查看每次commit信息，git的后悔药
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-a0d6d83ada047ae9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/15009210-c2b76185c1678156.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/15009210-688730425c240bd6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 版本回退
每次`commit`都会有一个`commitID`记录这次提交的版本，`HEAD`是最新的版本，上一个版本为`HEAD^`,上100个版本为`HEAD~100`
```
git reset --hard commit_id    #回退到特定提交，需要知道commit_id，commit_id可以不用写完整，Git会自动查找，写前面几个足够识别的就可以。
git reset --hard HEAD~1    #回退到上一次提交
git reset --hard 1094a    #当回退反悔之后，如果当前窗口未关闭，还可以找到之前的commitID,就可以再退到特定版本，往前往后都可以
```
![image.png](https://upload-images.jianshu.io/upload_images/15009210-c868552e671d09d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 撤销修改
```
git reset HEAD <file>      #把暂存区的修改回退到工作区
git checkout -- <文件名.后缀>  #特定文件在工作区的修改全部撤销，即让这个文件回到最近一次git commit或git add时的状态，此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件

#丢弃本地修改，用远程库覆盖
git fetch origin   #先拉取
git reset --hard origin/master      #将你本地主分支重置到远程库主分支
```
## 删除文件
```
 #从版本库（本地仓库）中删除某个特定文件
git rm file   #下一次提交时，该文件就不再纳入版本管理了，如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f（译注：即 force 的首字母）
git rm -f file   #删除已暂存即add的文件
git commit -m "remove file"
```
```
#用版本库里的版本替换工作区的版本，相当于一键还原
git checkout -- test.txt
```
## 修改名称
```
git mv file_from file_to     #对文件改名
```
# 远程仓库
***
## 建立关联
```
ssh-keygen -t rsa -C "youremail@example.com"       #创建SSH Key,然后添加到github远程仓库
#git remote add origin <server>
git remote add origin git@github.com:youname/name.git
```
## 本地推送
```
#git push <仓库名称> <分支名称>
git push -u origin master  #当前分支master推送到远程，第一次需要-u来关联本地和远程库，后面就不用了
 git push origin master
```
## 克隆仓库
```
#git clone username@host:/path/to/repository
git clone git@github.com:yourname/name.git   #从本地克隆已有的远处仓库
```
# 分支关联
***
在你创建仓库的时候，master 是“默认的”。在其他分支上进行开发，完成后再将它们合并到主分支上。
## 创建和合并分支
```
git checkout -b dev  #git checkout命令加上-b参数表示创建并切换
git branch dev   #创建分支
git checkout dev    #切换分支
git branch   #查看当前分支
git merge dev     #合并指定分支到当前分支
git branch -d dev #删除分支
```
## 解决冲突
```
git merge dev     #合并时可能会有冲突，有冲突之后要手动解决冲突
git add readme.txt    #解决掉冲突之后添加到暂存区
git commit -m "conflict fixed"      #然后提交
git log --graph --pretty=oneline --abbrev-commit   #查看分支的合并情况
git log --graph  #可以看到分支合并图
```
## 分支策略
1. master分支应该是非常稳定的，仅用来发布新版本，平时不能在上面工作
2. 工作都在各自的dev分支上，dev分支是不稳定的，开发完毕时，再把dev分支合并到master上，在master分支发布1.0版本

工作流：
```
 git checkout -b dev    #创建并切换到新分支
 git add readme.txt    #在工作分支修改之后添加到暂存区
 git commit -m "add merge"     #然后提交到仓库
git checkout master #要合并时切换到主分支
git merge --no-ff -m "merge with no-ff" dev      #合并分支，使用--no-ff表示禁用Fast forward（不能查看合并情况），-m参数，把commit描述写进去
git log --graph --pretty=oneline --abbrev-commit   #查看分支历史
git branch -d dev   #合并完毕后，删除分支
```
通常，合并分支时，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息
强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息，从而可以查看合并分支图
## 修复BUG
一般来说，修复BUG我们也会新建一个BUG分支，修复完毕之后合并主分支，如果你的工作分支上还有工作没做完但是不能commit的，可以暂存。
流程：
```
git status   #来BUG了，查看当前仓库状态，有没有没提交的
git stash    #把当前工作现场“储藏”起来
git checkout master   #切换到需要修复BUG的分支
git checkout -b issue-101  #创建并切换BUG分支
git add readme.txt   #修复BUG后提交暂存区
git commit -m "fix bug 101"   #提交仓库
git checkout master     #修复完成，切换分支
git merge --no-ff -m "merged bug fix 101" issue-101 #合并BUG分支和要修复的分支
git checkout dev     #切换到工作分支
git stash list         #查看工作存储
git stash pop      #恢复工作现场的同时把stash内容也删了
git stash apply      #恢复工作现场（git stash pop命令的分解1）
git stash drop       #删除stash（git stash pop命令的分解2）
git stash apply stash@{0}       #恢复指定的工作现场
```

## 新功能分支
新功能开发流程：
```
git checkout -b feature-vulcan     #创建并切换新功能分支
git add vulcan.py     #开发完毕后添加暂存区
git status     #查看当前仓库状态，确认文件
git commit -m "add feature vulcan"   #提交仓库
git checkout dev    #切换工作分支，准备合并
git branch -d feature-vulcan       #删除新功能分支
git branch -D feature-vulcan       #没有合并的分支要删除需要用-D
```
## 团队协作
团队协作流程
```
git remote  #查看远程库信息，当前是哪个分支
git remote -v   #查看详细信息，显示了可以抓取和推送的origin的地址
git clone git@github.com:youname/name.git     #克隆分支
git checkout -b dev origin/dev    #创建远程工作分支到本地（在本地创建和远程分支对应的分支）
git add env.txt     #修改之后添加到暂存区
git commit -m "add env"       #提交到本地仓库
#git push origin <branch>
git push origin master #推送主分支
 git push origin dev #推送其他分支
git pull     #推送过程有冲突，先把最新的提交从origin/dev抓下来，在本地合并，解决冲突，再推送
git diff <source_branch> <target_branch>     #查看分支不同
git branch --set-upstream-to=origin/dev dev   #没有指定本地dev分支与远程origin/dev分支的链接时，设置dev和origin/dev的链接（建立本地分支和远程分支的关联）
git pull     #设置之后继续拉取
git add env.txt  #有冲突之后解决冲突，添加暂存区
git commit -m "fix env conflict"     #提交仓库
git push origin dev    #推送
git rebase      #如果本地未push的分支提交分叉太多，可以把本地未push的分叉提交历史整理成直线之后再提交
```
# 标签管理
***
发布一个版本时，我们通常先在版本库中打一个标签（tag），标签也是版本库的一个快照，标签（tag）就是一个让人容易记住的有意义的名字（如v1.2等），它跟某个commit绑在一起，更方便管理查找。
## 创建标签
创建标签流程：
```
git branch #查看当前的分支
git checkout marster  #切换到要打标签的分支
git tag v1.0   #打标签
git tag #查看打的标签
git log --pretty=oneline --abbrev-commit    #当忘记打标签时，查找历史commit
git tag v0.9 f52c633 #对历史commit打标签
git show v0.9 #查看标签信息
git tag -a v0.1 -m "version 0.1 released" 1094adb     #打带有说明的标签，用-a指定标签名，-m指定说明文字
```
## 操作标签
```
git tag -d v0.1 #删除标签
git push origin v1.0    #打的标签是在本地，需要推送到远程仓库git push origin <tagname>
git push origin --tags    #次性推送全部尚未推送到远程的本地标签

#标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除
git tag -d v0.9 
git push origin :refs/tags/v0.9      #推送删除标签git push origin :refs/tags/<tagname>
```
# 使用github
***
一般使用GitHub作为免费的git远程仓库，可以管理自己的仓库，也可以参与别的开源项目。
参与github上开源项目流程：
1. 在开源项目主页点`fork`，在自己的账号下克隆一个开源项目仓库
2. 从自己的账号下克隆下这个开源项目仓库
```
git clone git@github.com:youname/name.git
```
3. 修改自己账号下的开源仓库
4. 推送pull request给官方开源项目仓库来贡献代码

# 自定义git
***
一些有用的命令：
```
git config --global color.ui true     #彩色的 git 输出
git config format.pretty oneline      #显示历史记录时，只显示一行注释信息
```
## 忽略特殊文件
在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件，提交的时候就不会把这些文件提交上去。
官方的`.gitignore`文件模板：https://github.com/github/gitignore
`.gitignore`需要也提交到Git，检验.gitignore的标准是git status命令是不是说working directory clean。
`.gitignore`例子：
```
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```
```
git add -f App.class     #-f表示强制添加文件，忽略ignore规则
git check-ignore -v App.class     #检查gitignore规则，是否忽略了文件
```
## 配置命令别名
```
git config --global alias.st status    #git status 别名：git st
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"     #git log 设置颜色别名：git lg
```
--global参数是全局参数，配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用
查看最近一次提交：
```
git log -1
```
删除别名：
每个仓库的Git配置文件都放在.git/config文件中，可以在配置文件中删除别名，别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。配置别名也可以直接修改这个文件。



参考链接：
官网：https://git-scm.com/
中文网：http://www.git-scm.com.cn/1511.html
Git简易指南：http://www.bootcss.com/p/git-guide/
Git命令手册：https://git-scm.com/docs
Git教程：https://www.liaoxuefeng.com/wiki/896043488029600