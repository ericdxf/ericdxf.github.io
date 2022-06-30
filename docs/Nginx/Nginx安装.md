---
title: Nginx部署
category: Nginx
date: 2022-04-22 19:45:53
tags: 部署
---
### Nginx部署

##### 1.安装依赖包

```
//一键安装上面四个依赖
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```

```
yum -y install zlib zlib-devel --skip-broken
```



##### 2.下载并解压安装包

```
//创建一个文件夹
cd /usr/local
mkdir nginx
cd nginx
//下载tar包
wget http://nginx.org/download/nginx-1.18.0.tar.gz
tar -xvf nginx-1.18.0.tar.gz
```

##### 3.安装nginx

```
//进入nginx目录
cd /usr/local/nginx
//进入目录
cd nginx-1.18.0
//执行命令
./configure
//执行make命令
make
//执行make install命令
make install
```

##### 4.配置nginx.conf

```
// 打开配置文件
vim /usr/local/nginx/conf/nginx.conf
```

##### 5.启动命令

```
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

##### 6.添加到环境变量

```
ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/
```

- /usr/local/bin/就是环境变量目录