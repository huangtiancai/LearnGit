git is a distributed version control system
Git is free software

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
这个命令通常用来回答两个问题：
- 当前做的哪些更新还未暂存？
- 有哪些更新已经暂存还未提交？
`git status`通过列出文件名的方式回答了这个问题，`git diff`将通过文件补丁的格式显示具体哪些行发生了变化

修改
