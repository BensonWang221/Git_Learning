# Git_Learning
![image](https://user-images.githubusercontent.com/65100398/152174761-1664b780-d31e-4798-a97e-f1caea1f05e1.png)

# 基本指令
编辑区(增删改）->(git add) 暂存区 ->(git commit -m "comment") 本地仓库

查看commit记录: git log [option]
可在~/.bashrc中alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'再source ~/.bashrc

代码回退: git reset --hard commitId

查看所有记录: git reflog(代码回退后仍可以看到所有的commit记录）

设置git不管理哪些文件: 创建.gitignore文件，在此文件中写入不需要git管理的文件名
文件 .gitignore 的格式规范如下：

所有空行或者以 # 开头的行都会被 Git 忽略。

可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。

匹配模式可以以（/）开头防止递归。

匹配模式可以以（/）结尾指定目录。

要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（**）表示匹配任意中间目录，比如 a/**/z 可以匹配 a/z 、 a/b/z 或 a/b/c/z 等。

我们再看一个 .gitignore 文件的例子：

 忽略所有的 .a 文件
*.a

 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

 忽略任何目录下名为 build 的文件夹
build/

 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf

# 分支
查看分支: git branch

创建分支: git branch 分支名

切换分支: git checkout 分支名

创建并切换到分支: git checkout -b 分支名

当前分支合并别的分支: git merge 分支名

删除某个其他分支: git branch -d 要删除分支名(会做各种检查，可能删不掉，例如被删的分支上有改动还没有merge时)
               git branch -D 要删除的分支名(不做检查, 强制删除)

# 远程仓库
添加远程仓库: git remote add <远端名称> <仓库路径>
    远端名称: 默认是origin，取决于远端服务器设置
    仓库路径: 从远端服务器获取此URL

查看远程仓库: git remote

推送到远程仓库: git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名]]
    如果远端分支名和本地分支名名称一样，则只写一个分支名即可，如git push origin master:master -> git push origin master
    
    -f: 强制覆盖，当本地与远端有冲突时
    --set-upstream: 推送到远端的同时并建立起和远端分支的关联关系
    如果当前分支已经和远端分支建立关联，则可以省略分支名和远端名: git push

查看本地分支和远端分支的关联关系, 相当于更详细的git branch: git branch -vv
    如果要建立关联关系: --set-upstream
    
从远程仓库clone: git clone <仓库路径> [本地目录]
    如果已经有一个远程仓库，可以直接clone到本地；本地目录可以省略，会自动生成一个目录
    一般来说，同一个项目只需要clone一次

从远程仓库中拉取和抓取: 将远程仓库的更新下载到本地
    抓取命令: git fetch [remote name] [branch name]
        将远端仓库的更新都抓取到本地，不会进行合并
        如果不指定remote name和branch name，则抓取所有分支
    拉取命令: git pull [remote name] [branch name]
        将远端仓库的更改都拉到本地并自动进行合并，等同于fetch + merge
        如果不指定remote name和branch name，则抓取所有病更新当前分支


解决合并冲突: 再git push之前，先git pull拉取，在本地merge后再提交
