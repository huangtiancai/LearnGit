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
分支用法实操：
```bash
1.新建分支并切换该分支
$ git checkout -b iss1
Switched to a new branch 'iss1'

2.列出所有分支，带 * 的是当期分支
$ git branch
* iss1
  master
现在就可以在iss1分支上就行提交
3.
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

