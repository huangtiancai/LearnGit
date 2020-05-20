参考：https://www.jianshu.com/p/fe038a97bb3c

### 创建版本库
1. 创建本地仓库：
- mkdir learnGit
- cd learnGit
- git init     初始化

2. 克隆项目（远程仓库已经存在）
- git clone ...


### 工作流
1. Working Directory 工作目录
   git add files / get reset -- files
2. Stage(Index)      暂存区（预期下一次的提交）
   git commit / git checkout -- files
3. History           保存提交历史 

### 添加文件与提交更新
1. 添加文件和提交更新
- vim README.md   => 修改内容，无README.md会自动创建

2. 查看仓库状态
- git status  查看仓库状态
- git add <file> 跟踪文件，并将文件添加到暂存区
- git rm <file>  移除跟踪的文件
- git status     再运行git status查看仓库状态：显示README.md在暂存区中，将处于下一次提交的快照中
- git commit -m add "add README.md"  提交修改
```bash
$ git commit -m 'add README.md' 
[master (root-commit) 70ee8bf] add README.md  
1 file changed, 22 insertions(+)  
create mode 100644 README.md
```
- git status     提交后再查看仓库状态：可以看到当前没有需要提交的修改，且工作目录是干净的（working tree clean）
```bash
$ git commit -m 'add README.md' 
[master ac29c20] add README.md
1 file changed, 7 insertions(+), 1 deletion(-)
```
注意：每次修改文件都需要：`git add <file>` ,将修改过的文件添加暂存区，然后再提交:`git commit -m 'update README.md'`


3. 查看修改的内容
`git status`命令的输出可能比较模糊，如果需要知道具体修改了什么地方 => 使用 `git diff` 命令
这个命令通常用来回答两个问题：
- 当前做的哪些更新还未暂存？
- 有哪些更新已经暂存还未提交？
区别：
`git status`通过列出文件名的方式回答了这个问题；
`git diff`将通过文件补丁的格式显示具体哪些行发生了变化
`git diff`比较的是工作目录中文件和暂存区域内文件的差异，也就是修改之后还没有暂存起来的修改

提交修改和提交新文件步骤一样:
第一步是git add <file>;
第二部是git commit -m
合并：`git commit -a -m '描述信息'`
>git commit -a -m "<distriptive message>"只对已跟踪的文件有效，且一次提交所有已修改、未暂存的文件

有多种方法查看两次提交间的差异：
- git diff：工作目录与暂存区差异。如果文件还未被跟踪，则无法查看。
- git diff HEAD：工作目录、暂存区与当前分支最近一次提交（HEAD）的差异。
- git diff --staged：暂存区与HEAD间差异。在低版本的Git中，使用的是git diff --cached命令。
- git diff <commit>：当前目录与commit快照间差异

查看具体某个文件的变化：`git diff <file>` 或 `git diff -- <file>`  `--` 标记文件名称
`git diff --staged -- README.md`命令查看暂存区与HEAD间README.md差异。


提交后
```bash
$ git status
On branch master
nothing to commit, working tree clean

$ git diff
....无打印
```

### 分支
分支的用法：
```bash
git branch                   列出仓库中所有分支
git branch <branchname>      创建一个名为branchname的分支，但不会自动切换到这个分支
git checkout <branchname>    切换到分支
git checkout -b <branchname> 创建一个名为branchname的分支，同时切换到这个分支
git checkout -b <new_branch> <current_branchname>  在current_branchname 分支上创建一个名为new_branch的分支，同时切换到这个分支

git branch -d <branchname>   删除一个名为branchname的分支（安全操作：Git会阻止删除包含未合并更改的分支）
git branch -D <branchname>   删除一个名为branchname的分支（忽略未合并的更改，强制删除branch分支）
git branch -m <branchname2>  将当前分支重命名为branchname2
```

简单的分支新建、分支合并的例子：
需求：
1. 开发app
2. 为实现一个新的需求，创建一个分支。
3. 在刚创建的分支上开展工作

正在此时，接到通知需要立即修复一个很严重的bug，你将按照下面方式来处理：
1. 切换到你的线上分支（production branch）。
2. 为这个修补任务新建一个分支，并在其中修复bug。
3. 通过测试后，切换回线上分支，然后合并这个修补分支，最后推送到线上分支。
4. 删除修改bug创建的分支。切换回最初工作的分支上，继续工作

实操:
1. 新建分支并切换到该分支
$ git checkout -b iss1
Switched to a new branch 'iss1'

2. 列出所有分支，带 * 的是当前分支
$ git branch
* iss1
  master
现在就可以在iss1分支上就行提交

3. 修改文件
vim README.md

4. git commit -a -m '描述'
iss1分支随着工作的进展向前推进
![iss1分支](https://upload-images.jianshu.io/upload_images/3151492-b67e382a716e0de4.png?imageMogr2/auto-orient/strip|imageView2/2/w/800)

5. 在master分支上创建一个分支(注意和新建分支的区别)，用于修复bug，最后将该分支合并到master分支
注意：
在切换到其它分支前，需要确保当前分支工作目录、暂存区是干净的，否则可能会和你即将检出的分支产生冲突，从而阻止Git切换到该分支，可以通过git stash保存进度，也可以用git commit --amend修补提交。

6. 在master分支上创建一个`hotfix`分支，在该分支上解决问题
$ git checkout -b hotfix master
Switched to a new branch 'hotfix'

7. 修改文件
vim REANME.md

8. 在hotfix分支添加到暂存区并提交更新
git commit -a -m '描述'

![hotfix分支](https://upload-images.jianshu.io/upload_images/3151492-8098f7019221e27b.png?imageMogr2/auto-orient/strip|imageView2/2/w/800)



9. 合并分支，首先切换到需要合并的分支
git checkout master

10. 将hotfix合并到master
git merge hotfix
出现：Fast-forward 关键词

由于master分支指向的提交是hotfix分支提交的直接上游，所以，Git只是简单的将指针向前移动。
也就是说试图合并两个分支时，如果顺着一个分支走下去，能够到达另一个分支，那么Git在合并两者时，只会简单的将指针向前推进（右移），因为这种合并没有要解决的冲突conflict，称为快进fast-forward。

![合并](https://upload-images.jianshu.io/upload_images/3151492-d1fe71e097a9db33.png?imageMogr2/auto-orient/strip|imageView2/2/w/1016)

11. 现在切换到 `iss1` 分支
git checkout iss1

12. 再编辑，添加暂存，提交更新
vim README.md
git commit -a -m '描述'

13. 检出master
git checkout master

14. 将iss1与master合并
git merge iss1
一般这里会直接自动合并

注意这里与第10步不同 => 三方合并
![三方合并](https://upload-images.jianshu.io/upload_images/3151492-f0755700531534b4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1011)

15. 解决合并冲突
如果对同一个文件的同一个部分进行了不同的修改，Git在合并时将产生冲突conflict，不能自动合并
但是没有自动创建一个新的合并提交。Git会暂停下来，等待你去解决合并过程中产生的冲突

使用 git status 查看因包含冲突而处于未合并（unmerged）状态的文件
使用 git diff 查看具体冲突部分

16. 打开冲突文件
vim README.md
手动解决冲突部分，同时删除 <<<<<、=======、>>>>>>> 这些内容



### 储藏


### 同步更改
#### 如何同步修改
1. 远程分支
列出所有远程主机
 `git remote`
origin

这里的origin就是主机

查看远程主机网址：
`git remote -v`
origin  https://github.com/huangtiancai/LearnGit.git (fetch)
origin  https://github.com/huangtiancai/LearnGit.git (push)

查看主机详细信息：
`git remote show origin`

添加远程主机：
`git remote add <主机名> <网址>`

修改远程主机名称：
`git remote rename <原主机名> <新主机名>`

2. 取回更新
2.1 将远程主机的指定分支更新取回本地，忽略分支名则取回所有分支的更新：
`git fetch <远程主机名> <分支名>` => `git fetch origin master`

-r选项用以标记查看远程分支，-a选项标记查看所有分支：
`git branch -r`
  origin/master

`git branch -a`
  iss1
* master
  remotes/origin/master

2.2 在取回的远程主机`origin/master`基础上，创建一个新的分支`newBranch`
`git branch -b newBranch origin/master`

在本地分支上合并远程分支
`git merge origin/master`

2.3 取回更新同时与本地分支合并

$ git fetch origin <远程分支名>
$ git merge origin/<远程分支名>

取回指定远程主机的指定分支的更新，再与本地的指定分支合并（上面两个命令的合并）
最全命令形式：`git pull <远程主机名> <远程分支名>:<本地分支名>`

- 远程分支与指定分支合并，这里以远程master分支 合并到 本地master分支为例
  如：`git pull origin master:master`
- 如果远程分支与当前分支合并，则本地分支名部分可以省略，如：`git pull origin master`
- 如果当前分支与远程分支存在追踪关系，则可以省略远程分支名，如：`git pull origin`
- 如果当前只有一个追踪分支，则主机名也可以省略，即：`git pull`
- 如果使用git clone创建的仓库，Git会自动在本地分支master和远程分支origin/master间建立追踪关系（tracking）

- Git也允许手动建立追踪关系：
`$ git branch --set-upstream-to <远程主机名>/<远程分支名> <本地分支名>`
如果省略了最后一项的<本地分支名>，则默认将当前本地分支与指定远程分支建立追踪关系

2.4 推送更新到远程仓库
git push用于将本地分支更新，推送到远程仓库（格式与git pull有些像）
分支推送、拉取顺都是<来源地>:<目的地>
git pull：`git pull <远程主机名> <远程分支名>:<本地分支名>`
git push：`git push <远程主机名> <本地分支名>:<远程分支名>`

最全命令形式：`git push <远程主机名> <本地分支名>:<远程分支名>`
- 如果省略远程分支名，则表示将本地分支推送至与其存在追踪关系的远程分支
- 如果该远程分支不存在，则会自动创建。即：git push <远程主机名> <本地分支>
  `git pull <远程主机名> <本地分支名>`
- 如果当前分支与远程分支间存在追踪关系，则本地分支和远程分支都可以省略
  `git push <远程主机名>`
- `-u`选项表示推送本地更新的同时，指定`origin`为默认主机：
  `git push -u origin <本地分支名>:<远程分支名>`

- 删除指定远程主机的指定远程分支
  `git push <远程主机名> --delete <远程分支名>`
- 推送时省略本地分支名等效于删除指定远程分支，因为这等同于同送一个空的本地分支到远程分支
  `git push <远程主机名> :<远程分支名>`


>不带任何参数的git push默认只推送当前分支，称为simple方式。
此外，还有matching方式,matching方式默认推送所有有追踪关系的本地分支。
Git 2.0版本之前，默认使用matching方式，现在默认使用simple方式。如需修改，使用如下命令：
```bash
$ git config --global push.default matching
$ git config --global push.default simple
```

将本地所有分支推送到远程主机，不考虑是否存在对应远程分支
 `git push --all <远程主机名>`

强制推送本地更新到远程主机(避免使用)
 `git push --force <远程主机名>`

>如果远程主机版本比本地版本更新，推送时Git会报错，要求先取回更新在本地合并，然后再推送到远程主机;
 这时，如果你一定要推送，可以使用--force选项。
 使用--force选项进行推送时，会导致远程主机上内容被覆盖，应尽量避免使用--force选项，除非你特别确定。


2. 具体应用实例
使用GitHub作我们的远程仓库，与本地仓库同步
在GitHub创建一个名为LearnGit的空仓库，与本地LearnGit仓库同步
连接GitHub仓库，还需要创建SSH Key，把公钥添加至GitHub

```bash
1.添加远程主机:
git remote add <主机名> <网址>
git remote add orgin https://github.com/huangtiancai/LearnGit.git

如果添加远程主机：
打印：`fatal: remote origin already exists.`

2.添加完成后，可以查看到远程主机网址（使用GitHub仓库地址）
$ git remote -v
origin  https://github.com/huangtiancai/LearnGit.git (fetch)
origin  https://github.com/huangtiancai/LearnGit.git (push)

3.推送master分支到远程仓库
$ git push -u origin master:master
打印如下
Everything up-to-date
Branch 'master' set up to track remote branch 'master' from 'origin'.

原因：工作目录好像是最新的，实际上还是有修改未提交暂存和提交更新，所以习惯每次推送前使用 `git status` 或 `git diff` 查看下状态
这里确实有修改文件，但是未暂存提交

4. git status / git diff

5. git commit -a -m 'update README.md'

6. 再次推送
$ git push -u origin master:master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 2.29 KiB | 586.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/huangtiancai/LearnGit.git
   ec9fd32..c2fe6ca  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.


````














  
   














