# GitTips

![image](https://user-images.githubusercontent.com/16644948/156517349-e2334b92-6ec8-4425-82ea-2cb4020f4163.png)

Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
![image](https://user-images.githubusercontent.com/16644948/156517378-21604027-5b70-44d6-a441-7eace78d9ce0.png)

#### command line user guide

$ git help -a


#### HEAD
```sh
You can think of the HEAD as the "current branch". When you switch branches with git checkout, the HEAD revision changes to point to the tip of the new branch.

You can see what HEAD points to by doing:
$ cat .git/HEAD
```

#### git branch -vv 
```sh
list all branches on the local system including the remote branch information if they are tracked
```

#### 回退到某一commit
```sh
git reset --hard a1a6954dcbc13a544c2e7428dfb1fb10ac9cd179
```

#### error: Your local changes to the following files would be overwritten by merge
```sh
方法一、stash
$ git stash
$ git commit
$ git stash pop
$ git status & fix confilct & git add & commit & push 

git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。
git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
git stash clear: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

方法二、放弃本地修改，直接覆盖

$ git reset --hard
$ git pull
```

#### 合并推荐方式
```sh
使用分支，保持主分支的整洁。这是我强烈推荐的一点，在分支进行提交，然后切到主分支更新(git pull —rebase)，再合并分支、推送。这样的流程会避免交叉合并的情况出现（不会出现共同祖先节点为多个的情况）。事实上，git合并操作让很多人感到不知所措的原因就是各种原因所产生的交叉合并问题，从而造成在合并的过程中丢失某些代码。保持主分支的整洁能够避免交叉合并的情况出现。
```

####  Git HEAD detached from XXX (git HEAD 游离) 解决办法
```sh
HEAD 处于游离状态时，我们可以很方便地在历史版本之间互相切换，比如需要回到某次提交，直接 checkout 对应的 commit id 或者 tag 名即可。
它的弊端就是：在这个基础上的提交会新开一个匿名分支
也就是说我们的提交是无法可见保存的，一旦切到别的分支，游离状态以后的提交就不可追溯了。
解决办法就是新建一个分支保存游离状态后的提交：

$ git branch -v    # 查看当前领先多少
$ git branch temp
$ git checkout temp     # 新建一个 temp 分支，把当前提交的代码放到整个分支
$ git checkout main-branch    # checkout回主分支
$ git merge temp    # merge 刚才创建的临时分支，把那些代码拿回来
$ git status & fix confilct & git add & commit & push 
$ git branch -d temp    # 删除临时分支

查看log, 确保当前HEAD指向main-branch
```

#### Clone project
```sh
git clone git://example.com/myproject
```

#### fetch remote branch 拉取远程分支
```sh
If daves_branch exists on the remote repository, but not on your local branch, you can simply type:
$ git switch daves_branch  [from version 2.23]
```

#### 如果你想快速的代上面的分支，你可以直接切换到那个分支：
```sh
git checkout origin/feature
git checkout -b dev  创建并切换到新分支  git branch 新建+ git checkout 切换
```

#### 新增远程项目
```
git remote add [remote repo name as you want] [remote repo address]
```

#### 查看远程仓库
```
git remote
```

#### 向远程仓库推送
```sh
git push [remote repo name 1] branchName
git push [remote repo name 2] branchName
```

#### 创建本地分支
```
git branch [branch name]  # 从当前所在分支的最新commit创建分支，不建立追踪关系
git branch --tack [branch name]  # 追踪当前分支的当时创建新分支
```

#### 如果你想快速的代上面的分支，你可以直接切换到那个分支：
```
git checkout origin/feature
```

#### diff远程仓库和本地仓库
```sh
git diff master origin/master
```

#### 切换分支
```
git checkout [branch name]
```

#### 切换本地分支
```
git checkout 分支名
```

#### 提交分支数据到远程服务器
```
git push origin <local_branch_name>:<remote_branch_name>
 ```

#### delete remote branch 删除远程分支
```
$ git push -d <remote_name> <branch_name>
$ git branch -d <branch_name>
$ git push origin :develop
```

#### 从已有的分支创建新的分支(如从master分支),创建一个dev分支
```
git checkout -b dev
```

#### 关联对于dev分支的跟踪
```
git branch --set-upstream-to=origin/dev
```

#### 取消对master的跟踪
```
git branch --unset-upstream master
```

#### 撤销本地仓库(未提交)的修改
```
git checkout fileName
```

#### 取消暂存(已执行git add）
```
git reset HEAD <filename>
```
 
#### 查看指定文件的历史版本
```
git log <filename>
```

#### see some abbreviated stats for each commit
```
 $ git log --stat
```

#### 回滚到指定commitID
```
git checkout <commitID> <filename>
```

#### 删除最后一次远程提交
* revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在；
* reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录
```
git revert HEAD
git push origin master
```

#### 远端即可创建新的分支my_remote_new_branch,提交本地修改
```sh
git push origin master:my_remote_new_branch
```

#### 当前分支的某个文件提交到另一个分支
``` 
  1.先在将当前把目标文件提交到remote repository, 记下commit-id, git log
  2.git checkout 目标分支
  3.git cherry-pick commit-id
  4.git push origin {目标分支}
```
 
#### 使用此标记来过滤合并提交以查看项目的历史记录
```
git log --oneline --no-merges
```

#### 改写上一个提交信息
```
git commit -v --amend
```

#### 输出酷炫的可视化日志
```
git log --pretty=oneline --graph --decorate --all
```

#### 如果想知道更改内容和更改者的相关简要说明，可以向git申请变更日志类似的文件
```
git shortlog <commit>..HEAD
```

#### 查看特定日期范围的日志
```
git log --since= FEB 10 2016  --until= FEB 19 2016
```

#### 列出所有git别名
```
git config -l | grep alias | sed  s/^alias.//g
```
 
#### 搜索包含关键字的提交
```
git log -S"config.menu_items"
```

#### 多人合作开发 
```sh
情景: A同学B同学都在下班之前pull了master分支的最新代码
A同学 昨天加班加点在master分支提交了自己的成果
B同学今天早上来也勤勤恳恳地工作, 正打算提交, 怎么保证自己的代码提交了, 又不会冲掉A同学昨晚的提交呢?

步骤一:
git add -u      // B同学把本地有过更新的文件提交到staging

步骤二:
git commit -m "关于本次提交的描述"        // 把上一步的文件提交到local repository

步骤三:
git pull origin master // 假设当前在master分支, 也就是说把A同学的提交pull到本地workspace

步骤四:
此时会看到提示说有些文件存在冲突,也就是A同学和B同学都更改了同一文件的同一块内容, 此时B同学如果不确定, 需要和A同学沟通保留谁的更改

步骤五:
git diff HEAD    // 此时B同学通过该命令可以看到, 自己的文件由于步骤四的合并, 和 Local repository不一样, 又需要提交了

步骤六:
git add -u
git commit -m "关于本次提交的描述"
git push origin master   // 如果其他同学没有在你操作步骤四~六期间提交代码, 应该成功提交到Remote Repository, 并且保留了两位同学的提交
```

#### Windows提示fatal: Authentication failed
```s
设置>凭据管理器 删除gitlab保存的凭据，再次pull/clone会提示输入用户名密码
```
取消cherry-pick
```s
git cherry-pick --abort
```

#### 更新密码
```sh
git config --global --unset user.password
```

#### 正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，突然想转到其他分支上进行一些工作。但又不想提交进行了一半的工作，否则以后无法回到这个工作点。git stash用于将这些修改保存到本地。
```s
stash暂存文件的范围
1) 添加到暂存区的修改（staged changes）
2) Git跟踪的但并未添加到暂存区的修改（unstaged changes）
stash不会缓存的文件
1)* 在工作目录中新的文件（untracked files）
2) 被忽略的文件（ignored files）



给stash加message方便区分
git stash save "stash incomplete fix for bug#1"

重新应用缓存的stash（下面的指令用于将缓存堆栈中的第一个stash删除，并将对应修改应用到当前工作目录下）
git stash pop

多次将缓存堆栈中的stash应用到工作目录中，但不删除stash拷贝
git stash apply #默认为最近的stash
或通过名字指定
git stash apply stash@{1}

查看现有stash
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log

移除stash
git stash drop stash@{1}

删除所有缓存的stash
git stash clear

查看指定stash的diff
git stash show
git stash show -p

```

#### 遇到protocol 'https' is not supported
1. repo address之前有空格

#### 修改密码后terminal如何更新密码
```sh
git config --system --unset credential.helper
git config --global credential.helper store
然后再push就会提示输入用户名和新密码
```

#### 删除远程仓库文件/文件夹
```sh
rm file/folder (-rf)
git commit -a -m "A file was deleted"
git push
```

#### 删除远程分支
```sh
git push --delete origin [branch name]
```

#### 为已存在的仓库添加.gitignore文件
```sh
git ignore旨在对不在git仓库中的文件进行忽略，如果这些文件已经在git仓库中，则不会忽略。所以如果需要忽略的文件已经提交到本地仓库，则需要从本地仓库中删除掉，如果已经提交到远端仓库，则需要从远端仓库中删除。删除.gitignore文件才能实际生效。

步骤：
从远端仓库clone一份代码
使用git rm file/to/be/ignored -r 删除需要被忽略的文件
.gitignore中配置需要被忽略的文件
git add . 然后git commit ；再git push 到远端服务器
这样保证远端服务器上没有需要被Ignore的文件,即使在本地修改这些文件，使用git status查看也不会再有提示了。

## .gitignore示例
.idea
*.iml
/log

```

#### unstage staged changes for the given file
```sh
git reset -- <filePath>  # correct command

git rm --cached <filePath>  # not suggeust
does not unstage a file, it actually stages the removal of the file(s) from the repo (assuming it was already committed before) but leaves the file in your working tree (leaving you with an untracked file).
That said, if you used git rm --cached on a new file that is staged, it would basically look like you had just unstaged it since it had never been committed before.
```

#### delete local branch
```sh
git branch -D dev
```

#### attach file to repo
```sh
![alt text](http://url/to/img.png)

or if it's stored in your repository. i.e.
![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
```

#### 修改git编辑器从nano到vim
    git config --global core.editor vim
    
#### $ git commit -m'xxx'  error: insufficient permission for adding an object to repository database .git/objects
    $ sudo chown -Rv xiaoxubeii:xiaoxubeii .git/objects/*

#### 删除分支上最近一次的提交记录(删除commit)
```sh
$ git reset --hard HEAD^
$ git push origin AIX -f
```

#### 提交merge request(界面)
```sh
1. 从目标分支新建一个分支
2. 做出修改提交commit
3. 到目标分支选择merge request
4. 等待merge
```

#### download a single from from github repo

1. locate the file and click "Raw" to get the link
2. wget/curl the link to download the file
