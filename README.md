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
- git commit -m add "add distributed"  提交修改
- git status     提交后再查看仓库状态：可以看到当前没有需要提交的修改，且工作目录是干净的（working tree clean）
