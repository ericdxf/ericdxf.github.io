### 1、安装Mongodb

进入opt目录

```bash
cd /opt
```


wget下载压缩包

```bash
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.1.tgz
tar zxvf mongodb-linux-x86_64-4.0.1.tgz
```

将解压文件移动到安装目录

```shell
mv mongodb-linux-x86_64-4.0.1 /usr/local/mongodb
```

创建mongodb存放路径

```
mkdir -p /usr/local/mongodb/data/
```

创建mongodb日志文件存放文件

```
mkdir /usr/local/mongodb/logs/
cd logs/
vim mongodb.log
```

### 2、启动mongodb

```bash
/usr/local/mongodb/bin/mongod --port 27017 --fork --dbpath=/usr/local/mongodb/data/ --logpath=/usr/local/mongodb/logs/mongodb.log --logappend --bind_ip_all&
```

`bind_ip_all`配置是开启所有ip访问的配置

检查端口是否占用

```bash
netstat -lanp | grep "27017"
```


进入mongodb数据库控制台

```bash
./mongo
```

### 3、设置mongodb全局

添加环境变量

```bash
vi /etc/profile
```


同样使用VI编辑器，加入如下配置

```bash
export PATH=$PATH:/usr/local/mongodb/bin
```

使配置文件立即生效

```bash
source /etc/profile
```

然后就可以全局使用mongodb命令了
进入mongodb控制台

> mongo  #进入MongoDB控制台
>
> show dbs #查看默认数据库
>
> use admin  #切换到admin数据库
>
> exit #退出MongoDB控制台