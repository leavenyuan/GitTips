# GitTips

![image](https://user-images.githubusercontent.com/16644948/156517349-e2334b92-6ec8-4425-82ea-2cb4020f4163.png)

Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
![image](https://user-images.githubusercontent.com/16644948/156517378-21604027-5b70-44d6-a441-7eace78d9ce0.png)

***
#### 使用submodule将子项目作为父项目的子模块，单独在不同的分支上开发
1. 将内部子目录设置为 Submodule： 在外部目录中添加内部子目录作为 submodule。

`git submodule add <URL-to-internal-repository> internal_directory`

2. 切换到需要的分支： 在外部目录中切换到分支 A，然后初始化和更新 submodule。

`git checkout A`

`git submodule update --init --recursive`

3. 在内部子目录中工作： 切换到内部子目录，并在其中工作，可以自由切换到分支 B。

`cd internal_directory`

`git checkout B`

***
#### 如何解决 fatal: Not possible to fast-forward, aborting

当出现 "fatal: Not possible to fast-forward, aborting." 错误时，通常是因为 Git 无法执行快速转发合并（fast-forward merge），这可能是由于当前分支与目标分支之间存在冲突或历史提交记录的变化所致。

##### 法1: 拉取最新代码并重新尝试合并：首先确保您的本地仓库是最新的，并尝试再次执行合并操作。
`git fetch origin`

`git checkout <your-branch>`

`git merge origin/<target-branch>`

##### 法2: 强制执行合并：如果您确定合并操作是安全的，可以使用 --ff-only 选项来强制执行快速转发合并。
`git merge --ff-only origin/<target-branch>`

##### 法3: 重新设置当前分支：如果以上方法都不起作用，您可以尝试将当前分支重置为目标分支的最新状态。请确保您的工作区是干净的，并且没有未提交的更改。
`git checkout <your-branch>`

`git reset --hard origin/<target-branch>`
##### 法4: 手动合并：作为最后的手段，您可以手动合并更改。这涉及查看冲突并手动编辑文件以解决冲突。
`git merge origin/<target-branch>`

`git add .`

`git commit -m "Resolved conflicts"`


***
#### 查看哪些提交尚未合并到目标分支

`git log origin/target-branch..source-branch`

***
#### 将另一个分支的所有改动合并到当前分支
1. 步骤一：切换到目标分支

首先，确保你在当前分支上，然后使用以下命令切换到要从中合并改动的目标分支。例如，假设你想要将feature-branch分支的改动合并到当前分支：

`git checkout feature-branch`

2. 步骤二：拉取最新的远程改动（可选）

如果你的目标分支是基于远程仓库的，建议在合并之前拉取最新的远程改动：

`git pull origin feature-branch`

3. 步骤三：切换回当前分支

`git checkout your-current-branch`

4. 步骤四：合并分支
现在，你可以使用git merge命令将目标分支的改动合并到当前分支：

`git merge feature-branch`

这将会尝试自动合并改动。如果有冲突，Git 会在合并的过程中暂停，提示你手动解决冲突。在解决冲突后，使用git add命令将解决的文件标记为已解决，然后继续合并过程：

`git add <conflicted-file>`
`git merge --continue`

5. 步骤五：解决冲突（如果有）
如果合并过程中发生冲突，你需要解决冲突。Git 会在有冲突的文件中标记冲突的地方，你需要手动编辑这些文件，解决冲突，然后继续合并。

6. 步骤六：完成合并

完成合并后，你可以提交合并的改动：

`git commit -m "Merge changes from feature-branch"`

注意事项：

- 在执行合并之前，确保你的工作目录是干净的，没有未提交的改动。
- 在合并之前，建议先测试改动，确保它们在当前分支上正常工作。
- 如果你只是想查看要合并的改动而不实际进行合并，可以使用git diff命令来查看差异。

***
#### 从 dev 分支拉取个人开发分支、完成本地开发并合并到开发分支的典型流程
下面是一个从 dev 分支拉取个人开发分支、完成本地开发并合并到开发分支的典型流程，以及相关的 Git 命令：

1. 确保你在 dev 分支上：
git checkout dev

3. 拉取最新的 dev 分支代码：
git pull origin dev 

3. 创建并切换到个人开发分支（假设分支名为 feature-branch，你可以根据实际情况修改）：
git checkout -b feature-branch 

4. 进行本地开发：
在 feature-branch 分支上进行你的本地开发。

5. 提交本地开发的变更：
git add . git commit -m "描述你的提交信息" 

6. 推送变更到远程个人开发分支：
git push origin feature-branch 

7. 在 GitLab 平台上创建合并请求：
打开你的个人开发分支 feature-branch，然后选择创建合并请求。选择将变更合并到 dev 分支。这一步通常在远程仓库的网页界面上完成。

8. 等待审核并解决可能的冲突：
如果有其他开发人员审核你的合并请求，并且出现冲突，解决冲突并提交变更。

9. 合并到 dev 分支：
在远程仓库上合并合并请求，

或者在本地执行：
- 切换回 dev 分支  $ git checkout dev
- 拉取最新的 dev 分支代码 $ git pull origin dev 
- 合并 feature-branch 到 dev 分支 $ git merge feature-branch 
- 解决可能的冲突 
- 提交合并的变更 $ git commit -m "Merge feature-branch into dev" 
- 推送合并后的 dev 分支到远程仓库  $ git push origin dev

10. 删除本地的个人开发分支（如果不再需要）：
$ git branch -d feature-branch 
或者如果分支没有被完全合并，可以使用 -D 强制删除：
$ git branch -D feature-branch 

这就完成了从 dev 分支拉取个人开发分支，完成本地开发并合并到开发分支的整个流程。请确保在执行 Git 操作时理解每个命令的作用，并谨慎处理可能的冲突。

***
#### command line user guide

$ git help -a


***
#### HEAD
```sh
You can think of the HEAD as the "current branch". When you switch branches with git checkout, the HEAD revision changes to point to the tip of the new branch.

You can see what HEAD points to by doing:
$ cat .git/HEAD
```

***
#### git branch -vv 
```sh
list all branches on the local system including the remote branch information if they are tracked
```

***
#### 回退到某一commit
```sh
git reflog  # 找到提交id
git reset --hard a1a6954dcbc13a544c2e7428dfb1fb10ac9cd179
```

***
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

***
#### 合并推荐方式(git rebase)
[git rebase](https://tutorial.helloflask.com/database/)
```sh
使用分支，保持主分支的整洁。这是我强烈推荐的一点，在分支进行提交，然后切到主分支更新(git pull —rebase)，再合并分支、推送。这样的流程会避免交叉合并的情况出现（不会出现共同祖先节点为多个的情况）。事实上，git合并操作让很多人感到不知所措的原因就是各种原因所产生的交叉合并问题，从而造成在合并的过程中丢失某些代码。保持主分支的整洁能够避免交叉合并的情况出现。
```

***
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

***
#### Clone project
```sh
git clone git://example.com/myproject
```

***
#### fetch remote branch 拉取远程分支
```sh
If daves_branch exists on the remote repository, but not on your local branch, you can simply type:
$ git switch daves_branch  [from version 2.23]
```

***
#### 如果你想快速的代上面的分支，你可以直接切换到那个分支：
```sh
git checkout origin/feature
git checkout -b dev  创建并切换到新分支  git branch 新建+ git checkout 切换
```

***
#### 新增远程项目
```
git remote add [remote repo name as you want] [remote repo address]
```

***
#### 查看远程仓库
```
git remote
```

***
#### 向远程仓库推送
```sh
git push [remote repo name 1] branchName
git push [remote repo name 2] branchName
```

***
#### 创建本地分支
```
git branch [branch name]  # 从当前所在分支的最新commit创建分支，不建立追踪关系
git branch --track [branch name]  # 追踪当前分支的当时创建新分支
```

***
#### 如果你想快速的代上面的分支，你可以直接切换到那个分支：
```
git checkout origin/feature
```

***
#### diff远程仓库和本地仓库
```sh
git diff master origin/master
```

***
#### 切换分支
```
git checkout [branch name]
```

***
#### 切换本地分支
```
git checkout 分支名
```

***
#### 提交分支数据到远程服务器
```
git push origin <local_branch_name>:<remote_branch_name>
 ```

***
#### delete remote branch 删除远程分支
```
$ git push -d <remote_name> <branch_name>
$ git branch -d <branch_name>
$ git push origin :develop
```

***
#### 从已有的分支创建新的分支(如从master分支),创建一个dev分支
```
git checkout -b dev
```

***
#### 关联对于dev分支的跟踪
```
git branch --set-upstream-to=origin/dev
```

***
#### 取消对master的跟踪
```
git branch --unset-upstream master
```

***
#### 撤销本地仓库(未提交)的修改
```
git checkout fileName
```

***
#### 取消暂存(已执行git add）
```
git reset HEAD <filename>
```

*** 
#### 查看指定文件的历史版本
```
git log <filename>
```

***
#### see some abbreviated stats for each commit
```
 $ git log --stat
```

***
#### 回滚到指定commitID
```
git checkout <commitID> <filename>
```

***
#### 删除最后一次远程提交
* revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在；
* reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录
```
git revert HEAD
git push origin master
```

***
#### 远端即可创建新的分支my_remote_new_branch,提交本地修改
```sh
git push origin master:my_remote_new_branch
```

***
#### 当前分支的某个文件提交到另一个分支
``` 
  1.先在将当前把目标文件提交到remote repository, 记下commit-id, git log
  2.git checkout 目标分支
  3.git cherry-pick commit-id
  4.git push origin {目标分支}
```

***
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
git show {commit id} # 查看具体提交内容
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
```sh
1. locate the file and click "Raw" to get the link
2. wget/curl the link to download the file
```

#### 已经push到远程仓库的分支，如何使得gitignore文件生效
```sh
vi .gitignore
git rm --cached 文件路径
git add .gitignore
git commit -m "Update .gitignore"
git push origin 分支名
```



#### 要将两个文件夹的文件在一个git仓库中维护
```sh
创建不同的分支，维护不同的文件夹
cd 目录1
git init
git remote add origin 远程仓库URL
git add .
git commit -m "Initial commit"
git push origin master

cd 目录2
git init
git remote add origin 远程仓库URL
git add .
git commit -m "Initial commit"
git checkout -b 新分支名
git push origin 新分支名
```
