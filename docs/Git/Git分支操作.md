### git checkout
首先，我们创建dev分支，然后切换到dev分支：
```
PS C:\workspace\AtomSpace\MarkDowns> git checkout -b dev
Switched to a new branch 'dev'
```
需要注意的是，上述这种方式创建的分支是从当前分支迁出一个新的本地分支，提交之后推送的话会提示需要设置远程分支
```
D:\workspace\WebWorkspace\Lover (test -> origin)
λ git push
fatal: The current branch test has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin test
```
此时按照上面提示的添加--set-upstream origin `分支名`就可以了。  
如果想从远程直接迁出分支到本地的话，可以使用如下命令`git checkout -b 本地分支名 远程分支名(可通过git branch-a查看)`
```
[root@VM_94_149_centos Lover]# git checkout -b branch_du remotes/origin/branch_du
Branch branch_du set up to track remote branch branch_du from origin.
Switched to a new branch 'branch_du'
```
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

`git branch dev`
`git checkout dev`

### git branch
`git barnch`命令是查看分支状态，如下面的调用，显示的是所有的分支，以及当前所在的分支是 dev
```
PS C:\workspace\AtomSpace\MarkDowns> git branch
* dev
  master
```
想要查看远程分支的话，用`git branch -a`
```
PS C:\workspace\AtomSpace\MarkDowns> git branch -a
* dev
  master
  remotes/origin/dev
  remotes/origin/master
```
### git merge
dev分支上做的改动，在切换回master之后是看不到的，因为这是在这个分支上进行的改动。那么当我们切换回master后如何把分支上的代码合并过来呢，就是通过调用`git merge dev`这个命令了。
```
PS C:\workspace\AtomSpace\MarkDowns> git merge dev
Updating eb19384..079e10c
Fast-forward
 "Git/Git分支.md" | 18 ++++++++++++++++++
 1 file changed, 18 insertions(+)
```
合并了之后，再调用`git checkout master`切换回master后，就能在主干上看到所有改动了。
### git branch -d ***
分支合并了之后就会要删除不用的分支，这就是删除分支的指令了。
```
PS C:\workspace\AtomSpace\MarkDowns> git branch -d dev
Deleted branch branch_test (was 079e10c).
PS C:\workspace\AtomSpace\MarkDowns> git branch
* master
```
但是如果分支没有合并的话，直接删除会报错：
```
PS C:\workspace\AtomSpace\MarkDowns> git branch -d dev
error: The branch 'dev' is not fully merged.
If you are sure you want to delete it, run 'git branch -D dev'.
```
根据提示也可以看出来，删除没有提交的分支的话需要调用`git branch -D dev`来强行删除。
**注意：上面说的所有的分支操作都是在本地仓库进行的，只有在`git push`之后远程仓库才会有相应变化**

### git push origin -d \<branch name\>
那么如何删除远程的分支呢，需要调用这个指令`git push origin -d <branch name>`，下面是调用示例：
```
PS C:\workspace\AtomSpace\MarkDowns> git push origin -d dev
To https://github.com/ericdxf/Record.git
 - [deleted]         dev
```
### git 解决冲突
合并代码时难免会出现冲突，毕竟是多人协作搞出来的，难免出现大家修改一个文件的问题，这是会提示这个：
```
PS C:\workspace\AtomSpace\MarkDowns> git merge feature1

Auto-merging Git/readme.txt
CONFLICT (content): Merge conflict in Git/readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
此时在编辑器中能看到出现冲突的地方，在选择了留下哪部分代码解决了冲突之后，再进行`git add` `git commit`提交代码，就解决了这个冲突。

### git stash
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：
```
PS C:\workspace\AtomSpace\MarkDowns> git status
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   "readme.md"

no changes added to commit (use "git add" and/or "git commit -a")
```
并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
```
PS C:\workspace\AtomSpace\MarkDowns> git stash
Saved working directory and index state WIP on dev: 3182e2e 提交解决冲突操作
```
此时切换到发布分支上去checkout一个bug101分支，解决了bug之后，代码合并到主干上，然后把bug分支删除。此时应该切换回我们的dev开发分支上继续我们之前的开发：
```
PS C:\workspace\AtomSpace\MarkDowns> git checkout dev
Switched to branch 'dev'
PS C:\workspace\AtomSpace\MarkDowns> git status
On branch dev
nothing to commit, working tree clean
```
此时你会发现，之前写的没有提交的部分都不见了，因为被`git stash`命令储藏起来了，那如何把这些代码取出来呢，有两种方式：
* `git stash apply` + `git stash drop`
用git stash apply恢复了代码之后，stash内容并不删除，所以要再调用一次`git stash drop`命令来删除。   

但有一点要注意，**`drop`命令只会删除最上面一次stash，并不是清空，应该是利用弹栈的方式，需要注意。**
* `git stash pop`
这个命令就是上面两个命令的叠加了，取出存储的数据之后同时删除存储库中的数据。
* `git stash list`
这个命令可以查看当前存储的数量，如下：
```
PS C:\workspace\AtomSpace\MarkDowns> git stash list
stash@{0}: WIP on master: 3182e2e 提交信息1
stash@{1}: WIP on dev: 3182e2e 提交信息2
```

### git remote
要查看远程库的信息时，用`git remote`，远程库的名称默认为**origin**
```
PS C:\workspace\AtomSpace\MarkDowns> git remote
origin
```
`git remote -v`能查看更详细的信息，包括远程仓库的地址：
```
PS C:\workspace\AtomSpace\MarkDowns> git remote -v
origin  https://github.com/ericdxf/Record.git (fetch)
origin  https://github.com/ericdxf/Record.git (push)
```
`git remote add origin <url>`这个是添加远程仓库的指令，那想换新的仓库怎么办呢，很简单。首先移除远程仓库：
`git remote rm origin`之后再添加新的远程库就好了。
### git tag
命令`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个commit id；
`git tag -a <tagname> -m "blablabla...`可以指定标签信息；
`git tag -s <tagname> -m "blablabla...`可以用PGP签名标签；
命令`git tag`可以查看所有标签。
命令`git push origin <tagname>`可以推送一个本地标签；
命令`git push origin --tags`可以推送全部未推送过的本地标签；
命令`git tag -d <tagname>`可以删除一个本地标签；
命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。
