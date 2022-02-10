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

查看修改: diff
git diff: 尚未暂存的文件更新了哪些部分, 注意是工作目录的当前文件和暂存区快照之间的差异，也就是修改后还没有暂存起来的变化内容
git diff --staged: 对比已暂存文件与最后一次提交的文件差异

# 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 “Changes not staged for commit” 部分（也就是 未暂存清单）看到：

$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    PROJECTS.md

no changes added to commit (use "git add" and/or "git commit -a")
然后再运行 git rm 记录此次移除文件的操作：

$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    deleted:    PROJECTS.md
下一次提交时，该文件就不再纳入版本管理了。 如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 --cached 选项：

$ git rm --cached README
git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。比如：

$ git rm log/\*.log
注意到星号 * 之前的反斜杠 \， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 shell 来帮忙展开。 此命令删除 log/ 目录下扩展名为 .log 的所有文件。 类似的比如：

$ git rm \*~
该命令会删除所有名字以 ~ 结尾的文件。
 
# 移动文件
不像其它的 VCS 系统，Git 并不显式跟踪文件移动操作。 如果在 Git 中重命名了某个文件，仓库中存储的元数据并不会体现出这是一次改名操作。 不过 Git 非常聪明，它会推断出究竟发生了什么，至于具体是如何做到的，我们稍后再谈。

既然如此，当你看到 Git 的 mv 命令时一定会困惑不已。 要在 Git 中对文件改名，可以这么做：

$ git mv file_from file_to
它会恰如预期般正常工作。 实际上，即便此时查看状态信息，也会明白无误地看到关于重命名操作的说明：

$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
其实，运行 git mv 就相当于运行了下面三条命令：

$ mv README.md README
$ git rm README.md
$ git add README
如此分开操作，Git 也会意识到这是一次重命名，所以不管何种方式结果都一样。 两者唯一的区别在于，git mv 是一条命令而非三条命令，直接使用 git mv 方便得多。 不过在使用其他工具重命名文件时，记得在提交前 git rm 删除旧文件名，再 git add 添加新文件名。

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
