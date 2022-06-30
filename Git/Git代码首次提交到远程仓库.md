---
title: Git代码首次提交到远程仓库
category: Git
date: 2017-08-30 17:45:53
tags: Git基础
---

很多人接触Git后的第一步操作就是提交本地的代码到远程仓库，包括之前是使用SVN等其他版本控制工具的用户。我们这里就来详细介绍一下如何把本地的代码关联到远成仓库。

首先，要有一个远程仓库，其实按照Git的分布式原理，任何一台电脑都可以作为一个远程仓库。但在实际的使用中，往往需要这个远程仓库保持全天24小时不停地运行，不然不能保证多人协作下的实时性。但是每个项目都单独搭建这么一台仓库也是费时费力的，好在前辈们已经早已想到了这个问题，GitHub就是这样一个大型的代码仓库，全世界无数的代码都托管在上面，类似的还有GitLab、码云等。当然托管上去后，你的代码就可以被所有人查看了。可以通过付费的方式设置为私有的，这个就要量力而行了。
1. 好了，貌似有些跑偏了，现在就先在GitHub上新建一个远程仓库：

![test](/assets/images/git_new_repository.png)
之后填写仓库的信息：
![test](/assets/images/git_new_repository_1.png)
仓库创建好了之后就可以把仓库的地址拷出来备用了：
![test](/assets/images/git_new_repository_path.png)

2. 然后就是打开你的项目所在目录，要把本地的代码提交到远程仓库有两种方法，第一种是先把远程仓库克隆到本地，再把代码拷进去，然后提交代码。但是这种方法不推荐，更推荐的方法是先在本地代码所在路径建立一个本地仓库,Git支持把远程仓库和一个已有的本地仓库关联。我们详细介绍一下这个方式：
首先进入项目的根目录，执行命令`git init`,生成本地仓库：
```
PS E:\respositoryTest> git init
Initialized empty Git repository in E:/respositoryTest/.git/
```
3. 然后把需要提交的代码关联到本地仓库中，当然前提是要先提交正确的`ignore`文件，这个文件后期也可以修改，但是会麻烦一些，在另一篇文章中有详细的介绍。执行命令`git add .`:
```
PS E:\respositoryTest> git add .
```
4. 接下来就是关联本地仓库到远程仓库，执行命令`git remote add origin <url>`，其中的url就是上面再GitHub中获取到的远程仓库地址：
```
PS E:\respositoryTest> git remote add origin https://github.com/ericdxf/test1.git
```
5. 接下来要先把远程仓库中的代码同步一下，虽然是个空仓库，但也有一些`README.md` `ignore`文件等，不同步的话等下的提交会报错的：
```
PS E:\respositoryTest> git pull origin master --allow-unrelated-histories
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/ericdxf/test1
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
```
可以看到更新下来了三个文件，其实更新代码只要`git pull origin master`就好了，但是首次更新的话需要添加` --allow-unrelated-histories`后缀，不加的话会报错`fatal: refusing to merge unrelated histories`,这个问题是在Git 2.9.0之后的版本才出现的，以前的版本不加也可以正常工作，具体原因应该是为了增强安全性而加的处理吧。  

6. 接下来是把代码提交到本地仓库中，调用`git commit -m 提交信息`，提交信息就是你的这次改动的内容；一定要认真填写，不然版本多了之后想再查询之前的提交信息时，就会很麻烦。
```
PS E:\respositoryTest> git commit -m "first commit"
[master 7e2adca] first commit
 1 file changed, 4 insertions(+)
 create mode 100644 try.txt
```
7. 最后一步就是把本地仓库的代码提交到远程仓库中，`git push origin master`,`master`是分支的名字，没有分支的话，默认的就是这个名为`master`的主干了。
```
PS E:\respositoryTest> git push origin master
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/ericdxf/test1.git
   f715821..7e2adca  master -> master
```
![end](images/git_new_repository_success.png)

好了，到这里，代码就关联到了GitHub中。 \^\_\^
