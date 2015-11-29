## git使用简易指南
> 参考
> [http://www.bootcss.com/p/git-guide/](http://www.bootcss.com/p/git-guide/)
> [http://blog.jobbole.com/53573/](http://blog.jobbole.com/53573/)

### 下载
下载git： [http://git-scm.com/download/](http://git-scm.com/download/)

### 注册账号
到这里注册账号[https://github.com/](https://github.com/)

### 设置git
配置git当前使用的用户名和邮箱，便于跟踪修改人
```
git config --global user.name "your_username"
git config --global user.email your_email@domain.com
```
设置默认推送，主要是在提交代码时可以将修改提交到默认的分之
```
git config --global push.default simple
```

### 工作流
你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `缓存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，指向你最近一次提交后的结果。


### 创建新仓库
创建文件夹，打开，在文件夹中执行`git init`创建新的仓库



### 添加与提交代码
```
//添加
git add <filename>
git add * 
git add -A
git add -all

//提交
git commit -m "提交内容"
```
现在，你的改动已经提交到了 HEAD，但是还没到你的远端仓库。
通过下面命令查看当前工作目录状态
```
git status
```



### 推送改动
你的改动现在已经在本地仓库的 HEAD 中了。执行如下命令以将这些改动提交到远端仓库：
```
git push origin master
```
可以把 master 换成你想要推送的任何分支。 

如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：
```
git remote add origin <server>
//eg：git remote add origin https://github.com/moonye6/blog.git
```
如此你就能够将你的改动推送到所添加的服务器上去了。


### 分支
分支是用来将特性开发绝缘开来的。在你创建仓库的时候，master 是“默认的”。在其他分支上进行开发，完成后再将它们合并到主分支上。
查看分支
```
git branch
```

创建一个叫做“feature_x”的分支，并切换过去：
```
git checkout -b feature_x
```
切换回主分支：
```
git checkout master
```
再把新建的分支删掉：
```
git branch -d feature_x
```
除非你将分支推送到远端仓库，不然该分支就是 不为他人所见的：
```
git push origin <branch>
```

### 更新与合并
要更新你的本地仓库至最新改动，执行：
```
git pull
```
以在你的工作目录中 获取（fetch） 并 合并（merge） 远端的改动。
要合并其他分支到你的当前分支（例如 master），执行：
```
git merge <branch>
```
两种情况下，git 都会尝试去自动合并改动。不幸的是，自动合并并非次次都能成功，并可能导致 冲突（conflicts）。 这时候就需要你修改这些文件来人肉合并这些 冲突（conflicts） 了。改完之后，你需要执行如下命令以将它们标记为合并成功：
```
git add <filename>
```
在合并改动之前，也可以使用如下命令查看：
```
git diff <source_branch> <target_branch>
```

### 标签
标签
在软件发布时创建标签，是被推荐的。这是个旧有概念，在 SVN 中也有。可以执行如下命令以创建一个叫做 1.0.0 的标签：
```
git tag 1.0.0 1b2e1d63ff
```
1b2e1d63ff 是你想要标记的提交 ID 的前 10 位字符。使用如下命令获取提交 ID：
```
git log
```
你也可以用该提交 ID 的少一些的前几位，只要它是唯一的。

通过命令可以回退代码到指定时间点（数字就是你的提交id）
```
git checkout 085bb3bcb
```

### 替换本地改动
假如你做错事（自然，这是不可能的），你可以使用如下命令替换掉本地改动：
git checkout -- <filename>
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到缓存区的改动，以及新文件，都不受影响。

假如你想要丢弃你所有的本地改动与提交，可以到服务器上获取最新的版本并将你本地主分支指向到它：
git fetch origin
git reset --hard origin/master

### 一个简单的流程示例
```
echo # blog >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:moonye6/PromiseB.git
git push -u origin master
```
> 这里可能碰到一个问题 就是按照这个操作了 还是提示远程分之不存在
> 那么需要确认两个问题 1 远程分支你是否有写入权限 咨询项目人就可以知道 2 删除掉本地项目.git/config下内容
> [remote "origin"]
>	url = git@github.com:moonye6/PromiseB.git
>	fetch = +refs/heads/*:refs/remotes/origin/*
>	重新执行
>	git remote add origin git@github.com:moonye6/PromiseB.git
> git push -u origin master
