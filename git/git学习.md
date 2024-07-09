# Git

## Git是什么

开源的**分布式版本控制系统**

## Git的相关理论

- Git的四大工作区域
  - workspace电脑本地看到的目录和文件
  - Index 暂存区 放在.git目录下
  - Repository 本地仓库 执行git clone就是把远程克隆到本地
  - Remote远程仓库
- Git工作流程
  ![image-20240307225516346](D:\deskstop\实习准备\实习\八股\git\image-20240307225516346.png)
- Git文件的四种状态
  - Untracked文件还没加入到git库，需要add变为Staged状态
  - Unmodified文件加入了git库，但是还没修改
  - Modified 文件被修改
  - stage暂存状态 通过git commit将修改同步到库中

## git的日常命令

- git clone
- git checkout -b dev
- git add
- git commit
- git log
- git diff
- git status
- git pull/git fetch
- git push

```bash
git clone url##克隆远程版本库到本地
git checkout -b [name] 创建开发分支，并切换到该开发分支下
git add ## 添加当前目录的所有文件到暂存区，包括子目录
git add [dir]## 添加指定目录到暂存区 包括子目录
git add [file1]##添加指定文件到暂存区

git commit -m [message]提交暂存区到仓库区
git commit [file1] -m [message]##提交暂存区的指定文件到本地仓库
git commit -amend -m [message]使用一次新的commit替代上一次提交

git status查看工作区状态
git status 查看工作区暂存区变动
-s是查看概要信息 --show-stash查看工作区中是否有暂存的文件

git log查看提交历史提交日志
--oneline精简模式   -p <file>查看指定文件的  <file>以列表方式查看指定文件提交历史

git diff显示暂存区和工作区的差异
+ filepath指定路径比较差异 HEAD filepath工作区与当前工作分支的比较差异 branchName filepth当前分支的文件与branckName分支文件的比较差异·1·1

git pull/fetch
git pull拉去远程仓库所有分支更新并合并到本地分支
.. origin master将远程master分支合并到当前本地分支
.. origin master:master 将远程master分支合并到当前本地master分支冒号后面是本地
git fetch --all拉取所有远端的最新代码
git fetch origin master一样

有些伙伴可能对使用git pull还是git fetch有点疑惑，其实 git pull = git fetch+ git merge。pull的话，拉取远程分支并与本地分支合并，fetch只是拉远程分支，怎么合并，可以自己再做选择。

git push推送本地分支到远程，也可以删除远程分支
git push origin master将本地分支更新全部推送到远程仓库master分支
git push origin -d <branchname> 删除远程branckname分支
git push --tags推送所有标签
```

## git进阶分支处理

```
git branch查看本地所有分支
.. -r查看远程所有分支
.. -a 查看所有远程和本地分支
.. -D <branchname>删除本地branchname分支

git checkout <>切换分支

我们在开发分支dev开发、测试完成在发布之前，我们一般需要把开发分支dev代码合并到master 
git merge master在当前分支上合并master分支过来
git merge --no-ff origin/dev  在当前分支上合并远程分支dev
git merge --abort 终止本次merge，并回到merge前的状态
```

## git进阶之处理冲突

情景 多个人修改同一处地方合并后会提示冲突

![image-20240308230859656](D:\deskstop\实习准备\实习\八股\git\image-20240308230859656.png)

## git进阶之撤销与回退

![image-20240308231310580](D:\deskstop\实习准备\实习\八股\git\image-20240308231310580.png)

git checkout
如果文件还在**工作区**，还没添加到暂存区，可以使用git checkout撤销修改

```
git checkout -- [file]  丢弃某个文件file的修改
git checkout .  丢弃所有文件
```

git reset

```
git reset HEAD --file回退暂存区里的某个文件，回退到当前版本工作区状态
git reset –-soft 目标版本号 可以把版本库上的提交回退到暂存区，修改记录保留
git reset –-mixed 目标版本号 可以把版本库上的提交回退到工作区，修改记录保留
git reset –-hard  可以把版本库上的提交彻底回退，修改的记录全部revert。
```

先看一个粟子demo吧，代码**git add到暂存区，并未commit提交**,可以酱紫回退，如下：

```
git reset HEAD file 取消暂存
git checkout file 撤销修改
```

再看另外一个粟子吧，代码已经git commit了，但是还没有push：

```
git log  获取到想要回退的commit_id
git reset --hard commit_id  想回到过去，回到过去的commit_id
```

如果代码已经push到远程仓库了呢，也可以使用reset回滚哦(这里大家可以自己操作实践一下哦)~

```
git log
git reset --hard commit_id
git push origin HEAD --force
```

git revert

![image-20240308233223881](D:\deskstop\实习准备\实习\八股\git\image-20240308233223881.png)

## git进阶之标签tag

打tag就是对发布的版本标注一个版本号，如果版本发布有问题，就把该版本拉取出来，修复bug，再合回去

```
git tag  列出所有tag
git tag [tag] 新建一个tag在当前commit
git tag [tag] [commit] 新建一个tag在指定commit
git tag -d [tag] 删除本地tag
git push origin [tag] 推送tag到远程
git show [tag] 查看tag
git checkout -b [branch] [tag] 新建一个分支，指向某个tag
```

git remote

```
git remote   查看关联的远程仓库的名称
git remote add url   添加一个远程仓库
git remote show [remote] 显示某个远程仓库的信息
```

git config --global --unset http.proxy 
git config --global --unset https.proxy