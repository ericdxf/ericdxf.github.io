---
title: MongoDB集群部署
category: MongoDB
date: 2022-05-16 18:45:53
tags: 中间件
---

参考链接

https://www.1024sou.com/article/809940.html

```
storage:
  dbPath: F:\MongoDB\Server\4.2\data
  journal:
    enabled: true

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path:  F:\MongoDB\Server\4.2\log\mongo.log

net:
  port: 27017
  # 允许所有IP地址进行访问
  bindIp: 0.0.0.0

#processManagement:

security:
  authorization: enabled
  # 用于副本集模式各节点互相认证用
  keyFile: C:\Program Files\MongoDB\Server\4.2\bin\keyFile\mongodb.key

#operationProfiling:

replication:
  # 副本名称
  replSetName: qdcares

#sharding:

## Enterprise-Only Options:

#auditLog:

#snmp:
```

查看当前副本集状态

```
rs.status()
```

```
# 创建登录账号
db.createUser(
  {
    user: "root",
    pwd: "mongo@2021",
    roles: [ { role: "root", db: "admin" } ]
  }
)  

# 创建登录账号
db.createUser(
  {
    user: "mongo",
    pwd: "mongo@2021",
    roles: [ { role: "readWrite", db: "qdcares" } ]
  }
)  

# 用户登录
db.auth("mongo","mongo@2021")
# 用户登录
db.auth("root","mongo@2021")
```

生成keyFile命令，Linux系统生成很方便，Windows没有尝试

```
sudo openssl rand -base64 741 >> /opt/mongodb.key
```

添加节点

生产环境添加节点时，建议将priority及votes设为0，即不会选为主（priority默认1），也没有投票特性（votes默认1，有投票权）

- priority：能否选为主节点

- votes：是否有投票权

```
test12:PRIMARY> rs.add( { host: "10.235.196.17:27017", priority: 0, votes: 0 } )
```

