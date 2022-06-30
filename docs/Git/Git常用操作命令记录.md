### git init
选择一个路径初始化代码版本库，目录下会多一个`.git`目录，这里面保存的是当前Git存储库保存的信息，用户要严格注意不要手动修改它。
### git add xxx.xxx
把xxx.xxx这个文件添加到git仓库，文件名对上就好，不需要索引相对路径。
### git add .
添加当前git仓库中所有的文件。
### git commit -m "提交信息"
提交所有有改动的文件到本地git仓库，包括新增和删除的文件。  
`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入`-m "xxx"`行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。
### git status
`git status`命令可以让我们时刻掌握仓库当前的状态，下面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
```
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```
从这些信息里能清楚地看出哪个文件有过改动，具体的改动信息呢就要通过下面这个指令来查看了。
### git diff

虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的readme.txt，所以，需要用`git diff`这个命令看看：
```git
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```
通过这个命令可以清晰的看到那些文件有了更改，包括新增删除或者修改等情况。
这里的`HEAD`指的是当前分支的上一次提交，而这句话的全部内容就是比较当前分支的状态和存储库中文件的区别。
```
git diff HEAD -- readme.txt
```
这句话的意思就是只单独比较这一个文件的差异。**HEAD\^代表上一次提交，\^符号的数量表示前几次提交。** 下面会有详细介绍。
### git checkout ***
当你对某个文件做了修改之后，有时难免会发现有些错误不该提交，需要撤销这些更改，就需要使用到这个命令了，还是先执行git status命令：
```
Your branch is up-to-date with 'origin/master'.
 
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
 
        modified:   "readme.md"
 
no changes added to commit (use "git add" and/or "git commit -a")
```
从上面的信息我们可以看出我对readme.md这个文件做了修改，也提到了`(use "git checkout -- <file>..." to discard changes in working directory)`，这句话的意思就是可以通过`git checkout -- <file>`命令来撤销某个文件的而更改:
```
PS C:\workspace\AtomSpace\MarkDowns> git checkout -- Git/ignore相关.md
```
执行了这条指令之后，就会发现这个文件已经撤销到了更改之前的状态。需要注意的是，分两种情况：
* 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

* 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。
还有一点就是如果不带 `--` 符号的话就是切换分支的指令了，需要注意一下。
### git reset
首先我们要先明确一下`Head`的概念：
**Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD\^，上上一个版本就是HEAD\^\^，当然往上100个版本写100个\^比较容易数不过来，所以写成HEAD~100。**
```
$ git reset --hard HEAD^
```
所以这句话的意思就是回退到上个版本了。回退和撤销有几种情况一起整理一下：
* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file。`

* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。  

### git rm ***.***
Git中的文件删除包括两种情况，一种是本地删除文件，然后提交修改到远程仓库，包括用户在资源管理器中进行删除；另一种是直接调用`git rm`命令进行删除。
* 第一种情况：
1. 在工作空间中新建一个`Others`文件夹，在其中新建一个test.txt文件，然后执行`git add Others/text.txt`添加到缓冲区，再`commit` `push`到远成仓库。
2. 本地删除该文件，比如在文件资源管理器中右键删除等，我这里是命令行删除的。  
3. 可以看出Git给出了提示，我可以通过`git add .`命令把文件删除操作提交到暂存区，之后推送到远程仓库。或者通过`git checkout Others/test.txt`命令来从暂存区还原这次删除。
4. 通过提示我们可以看出，本地删除的话只是把工作区间中的文件给删除了，暂存区和本地仓库，远程仓库中的文件都没有改变。  

```
PS C:\workspace\AtomSpace\MarkDowns> rm Others/test.txt
PS C:\workspace\AtomSpace\MarkDowns> git status
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    Others/test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
* 第二种情况  
通过调用`git rm Others/test.txt`命令来删除的话会删除本地文件，同时也会删除暂存区中的对应文件。我们直接看调用的情况，删除之后再调用一次`git status`命令来查看一下状态：  

```
PS C:\workspace\AtomSpace\MarkDowns> git rm Others/test.txt
rm 'Others/test.txt'
PS C:\workspace\AtomSpace\MarkDowns> git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
  
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        deleted:    Others/test.txt
```
可以看出提示我调用`git commit`来提交到远程仓库，再就是通过版本回退来恢复这次删除。这就证实了确实工作空间中的文件已经被删除了，同时暂存区中的文件也被删除了。  

**总结一下：第一种方式只会删除本地文件，暂存区，本地库，远程库的更新都需要手动操作；而第二种方式则会删除本地和暂存区中的文件，而本地库和远程库的更新需要再行操作**

### git clone
当你想从远程仓库克隆项目到本地的话，就可以通过调用`git clone`命令来实现，具体调用规则如下：
```
git clone https://github.com/ericdxf/test.git
```

参考资料：
廖雪峰大神的Git教程：[传送门](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
