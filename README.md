# Git_Learning
# 基本指令
编辑区(增删改）->(git add) 暂存区 ->(git commit -m "comment") 本地仓库

查看commit记录: git log [option]

代码回退: git reset --hard commitId

查看所有记录: git reflog(代码回退后仍可以看到所有的commit记录）

设置git不管理哪些文件: 创建.gitignore文件，在此文件中写入不需要git管理的文件名

#分支
查看分支: git branch

创建分支: git branch 分支名

切换分支: git checkout 分支名

创建并切换到分支: git checkout -b 分支名

当前分支合并别的分支: git merge 分支名

删除某个其他分支: git branch -d 要删除分支名(会做各种检查，可能删不掉，例如被删的分支上有改动还没有merge时)
               git branch -D 要删除的分支名(不做检查, 强制删除)
