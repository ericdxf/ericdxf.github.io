---
title: Redis集群部署
category: Redis
date: 2022-05-16 18:45:53
tags: 中间件
---

### 下载

下载地址： https://github.com/MicrosoftArchive/redis/releases

安装包已提供，如需下载则根据系统下载的版本：以（64位为例）

![202201051](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201051.png)

下载后一般解压到根目录下：如（E:\Redis-x64-3.2.100）

### 安装

打开cmd命令窗口，使用命令进行安装和注册redis到window服务

安装命令：redis-server.exe --service-install redis.windows.conf --loglevel verbose

启动服务命令：redis-server.exe --service-start

关闭服务命令：redis-server.exe --service-stop

![202201052](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201052.png)最后返回的successfully表示安装成功。

![202201053](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201053.png)

也可以通过任务管理器中查看后台中是否有redis-service来判断是否启动成功。

### 客户端使用redis

我们重新打开一个cmd ,作为一个客户端调用redis服务，如下图所示，调用命令是：redis-cli.exe -h [本机ip] -p 6379，如下图显示地址和端口，说明调用成功

然后我们使用set 和get 命令进行测试一下，set uname "abc",然后使用get uname可以获取到对应set的值，说明调用成功

![202201054](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201054.png)

![202201055](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201055.png)

### 设置密码

![202201056](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201056.png)

上图中的root可以改为自己设置的密码

还可以通过将字符串设置为空来清空密码：
CONFIG SET requirepass ""

### 问题记录：Creating Server TCP listening socket 127.0.0.1:6379: bind: No error

![202201057](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201057.png)

解决办法：

```
E:\Redis-x64-3.2.100>redis-cli.exe
127.0.0.1:6379> shutdown
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth root　## 登录
OK
127.0.0.1:6379> shutdown
not connected> exit

E:\Redis-x64-3.2.100>redis-server.exe redis.windows.conf
```

![202201058](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/202201058.png)