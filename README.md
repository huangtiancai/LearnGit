参考：https://www.jianshu.com/p/fe038a97bb3c

git的简单操作
1. 创建本地仓库：
- mkdir learnGit
- cd learnGit
- git init     初始化

2. 克隆项目（远程仓库已经存在）
- git clone ...

3.添加文件和提交更新
- vim README.md   => 修改内容，无README.md会自动创建

4. 查看仓库状态
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


5. 查看修改的内容
`git status`命令的输出可能比较模糊，如果需要知道具体修改了什么地方 => 使用 `git diff` 命令
命令：`git diff` 或 `git diff <file>`
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

提交后
```bash
$ git status
On branch master
nothing to commit, working tree clean

$ git diff
....无打印
```

6. 分支
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
手动解决冲突部分，同时删除 <<<<<、=======、>>>>>>> 这些






```

2020-05-19 13:40
Create a new branch, this sentence is edit at iss1.
iss1 分支随着工作的进展向前推进
删除分支 出现 HEAD detached at 1418300


2020-05-19 14:09
这里是在 master 分支创建的新分支 hotfix 上：
this is a sentence edited at hotfix.

2020-05-19 13:40
Git has a mutable index called stage

