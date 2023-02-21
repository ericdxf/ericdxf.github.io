## Git合并多次commit

有时候需要合并几个提交历史记录为一个提交，该怎么办呢？可以使用 `git rebase`

也可以使用 git reset --soft 或者 git reset --mixed 

#### 1.查看历史，确定要合并的提交

`git log 或者  git log --oneline `

> **注意：不要合并了其他人的提交，不要合并了其他人的提交，不要合并了其他人的提交**
>
> **ATTENTION: Do not merge others' commit! Do not merge others' commit! Do not merge others' commit!**

------

`git rebase -i ***`

其中 **\*\*\*** 为上一个不需要合并的 commit 的 hash 值。

也可以使用相对提交，例如我需要合并最近2次提交可以使用（倒数第三次之后的提交会合并，不合并第三次）

`git rebase -i HEAD^^^`

#### 2.编辑合并信息

- `pick`的意思是要保留这个 `commit`

- `squash`的意思是这个 `commit` 会被合并到前一个 `commit`

  整个过程是通过VIM编辑器操作的，修改完之后保存退出

![image-20230221102241195](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230221102241195.png)

若无冲突或者冲突已 fix，则会出现一个 **`commit message`** 编辑页面，修改 **commit message** ，然后保存退出。Successfully表示操作成功。

然后修改合并后的提交请求信息，继续保存后退出即可。

![image-20230221110339592](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230221110339592.png)



#### 3.中间可能会出现的情况


git 会压缩提交历史，若有冲突，需要进行修改，修改的时候保留最新的历史记录，修改完之后输入以下命令：

```bash
git add .
git rebase --continue
```

若想退出放弃此次压缩，执行命令：
`git rebase --abort`

#### 4.同步到远程 git 仓库

输入：`git push`  或者`git push -f`  或者 `git push --force`
查看远程仓库效果，多次 commit 已被合并成一次 commit。

![image-20230221110600722](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230221110600722.png)