---
title: Git提交时间修改
category: Git
date: 2022-05-16 18:45:53
tags: Git基础
---

## git提交时间修改

### 操作步骤：

- 下载修改提交时间插件`git-redate`

https://github.com/PotatoLabs/git-redate

- 解压文件夹，把git-redate文件置于git安装目录的\mingw64\libexec\git-core文件夹下
- 去到对应git项目的根目录下，执行如下命令：

```
git redate -c 6
```

数字6指的是查看最新的6条提交记录

- 出现如下界面：

![在这里插入图片描述](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/20210115174827502.png)

- 输入“1”按回车，进入vi视图界面。在这里可以看到git提交记录的相关信息包括时间

- 此时熟悉linux操作的同学应该知道怎么操作修改，不熟悉vi工具的同学可自行google学习基本操作命令。

简要操作：

1. 按“i”键，进入修改模式。

2. 找到对应记录的时间进行修改。

3. 修改完毕，按“ESC”键退出修改模式。

4. 按“:”键，输入wq保存修改。

   ![在这里插入图片描述](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/20210115175557762.png)

- 保存后，等待一会儿。出现如下提示，更新时间成功

  ![在这里插入图片描述](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/20210115175835886.png)

这里笔者这里会出现提示“line 50: ~/.redate-settings: No such file or directory”，可直接忽略不影响结果。

### 注意事项：

- 执行脚本前，请先确保所有修改内容已commit。否则会更新失败，出现如下提示：

![在这里插入图片描述](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/20210115180300103.png)

- 此方法只能修改提交记录的时间，不能修改提交的注释