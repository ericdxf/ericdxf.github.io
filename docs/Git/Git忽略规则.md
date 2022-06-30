---
title: Git忽略规则
category: Git
date: 2017-08-30 17:45:53
tags: Git基础
---

在git中如果想忽略掉某个文件，不让这个文件提交到版本库中，可以使用修改根目录中 .gitignore 文件的方法（如果没有这个文件，则需自己手工建立此文件）。这个文件每一行保存了一个匹配的规则例如：

```代码块
#此为注释 – 将被 Git 忽略
*.sample 　　 # 忽略所有.sample 结尾的文件
!lib.sample 　# 但 lib.sample 除外
/TODO 　　    # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/ 　　   # 忽略 build/ 目录下的所有文件
doc/*.txt 　　# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
### 提交代码之后修改ignore无效解决办法  
把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被追踪的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。<font color="#17da4d">那么解决方法就是先把本地缓存删除（改变成未被追踪状态），然后再提交：</font>  

git rm -r --cached .  
git add .  
git commit -m 'update .gitignore'  
### 检查忽略地址
有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被**ignore**忽略了：
```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.
```
你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：
```
$ git check-ignore -v App.class
.gitignore:3:*.class    App.class
```
Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。
### 最后附上官方的`.ignore`文件链接 [传送门](https://github.com/github/gitignore)
