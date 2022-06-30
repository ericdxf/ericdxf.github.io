---
title: MongoDB常用命令
category: MongoDB
date: 2022-05-16 18:45:53
tags: 中间件
---

### 启动Mongodb

```
./mongod -f /opt/mongodb/mongod.conf -fork
```

### 停止Mongodb

```
use admin;
switched to db admin
db.shutdownServer();
server should be down...
```

### 设置管理员账号

```
use admin  
db.createUser({
  user: 'root',  // 用户名
  pwd: 'qdcares805',  // 密码
  roles:[{
    role: 'root',  // 角色
    db: 'admin'  // 数据库
  }]
})
```

### 查询

```
db.sfss.find({"_id":ObjectId("60efeabf8036fc062411c4e7")})
db.fs.files.find({"_id":ObjectId("60efeabf8036fc062411c4e7")})
db.fs.chunks.find({"files_id":ObjectId("60efeabf8036fc062411c4e7")})
```

### 导出

```
./mongoexport -u root -p qdcares805 -q '{"_id":{ "$oid" : "60efeabf8036fc062411c4e7" }}' --authenticationDatabase admin -d qdcares -c sfss -o /home/mongod/mongo/database/export/sfss.json
```

### 导入

```
mongoimport.exe -u root -p qdcares805 --authenticationDatabase admin -d qdcares -c fs.files --type=json --file ../export/fs_files.json

mongoimport.exe -u root -p qdcares805 --authenticationDatabase admin -d qdcares -c fs.chunks --type=json --file ../export/fs_chunks.json

mongoimport.exe -u root -p qdcares805 --authenticationDatabase admin -d qdcares -c sfss --type=json --file ../export/sfss.json
```

### 图标资源导出路径记录

```
F:\project\mongo\icon
```

### 实例

```
// 图标文件
./mongoexport -u root -p qdcares805 -q '{"$or":[{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"_id":{"$oid":"6109f1868036fc05f049804d"}},
{"_id":{"$oid":"6109f2978036fc05f049804f"}},
{"_id":{"$oid":"6109f29c8036fc05f0498051"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"_id":{"$oid":"6109f1868036fc05f049804d"}},
{"_id":{"$oid":"60ffcb228036fc05f049802d"}},
{"_id":{"$oid":"60ffcb2a8036fc05f049802f"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"6109f32b8036fc05f0498053"}},
{"_id":{"$oid":"6109f32f8036fc05f0498055"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"6109f3b48036fc05f049805b"}},
{"_id":{"$oid":"6109f3b88036fc05f049805d"}},
{"_id":{"$oid":"6109f3738036fc05f0498057"}},
{"_id":{"$oid":"6109f3778036fc05f0498059"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"60ffcac28036fc05f0498021"}},
{"_id":{"$oid":"60ffcac68036fc05f0498023"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}}]}' --authenticationDatabase admin -d qdcares -c sfss -o /home/mongod/mongo/database/export/sfss.json



./mongoexport -u root -p qdcares805 -q '{"$or":[{"files_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"files_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"files_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"files_id":{"$oid":"6109f1868036fc05f049804d"}},
{"files_id":{"$oid":"6109f2978036fc05f049804f"}},
{"files_id":{"$oid":"6109f29c8036fc05f0498051"}},
{"files_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"files_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"files_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"files_id":{"$oid":"6109f1868036fc05f049804d"}},
{"files_id":{"$oid":"60ffcb228036fc05f049802d"}},
{"files_id":{"$oid":"60ffcb2a8036fc05f049802f"}},
{"files_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"files_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"files_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"files_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"files_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"files_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"files_id":{"$oid":"6109f32b8036fc05f0498053"}},
{"files_id":{"$oid":"6109f32f8036fc05f0498055"}},
{"files_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"files_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"files_id":{"$oid":"6109f3b48036fc05f049805b"}},
{"files_id":{"$oid":"6109f3b88036fc05f049805d"}},
{"files_id":{"$oid":"6109f3738036fc05f0498057"}},
{"files_id":{"$oid":"6109f3778036fc05f0498059"}},
{"files_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"files_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"files_id":{"$oid":"60ffcac28036fc05f0498021"}},
{"files_id":{"$oid":"60ffcac68036fc05f0498023"}},
{"files_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"files_id":{"$oid":"60efeac28036fc062411c4e9"}}]}' --authenticationDatabase admin -d qdcares -c fs.chunks -o /home/mongod/mongo/database/export/fs_chunks.json



// 全部
./mongoexport -u root -p qdcares805 -q '{"$or":[{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"_id":{"$oid":"6109f1868036fc05f049804d"}},
{"_id":{"$oid":"6109f2978036fc05f049804f"}},
{"_id":{"$oid":"6109f29c8036fc05f0498051"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"6109f17d8036fc05f049804b"}},
{"_id":{"$oid":"6109f1868036fc05f049804d"}},
{"_id":{"$oid":"60ffcb228036fc05f049802d"}},
{"_id":{"$oid":"60ffcb2a8036fc05f049802f"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"6109f32b8036fc05f0498053"}},
{"_id":{"$oid":"6109f32f8036fc05f0498055"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"6109f3b48036fc05f049805b"}},
{"_id":{"$oid":"6109f3b88036fc05f049805d"}},
{"_id":{"$oid":"6109f3738036fc05f0498057"}},
{"_id":{"$oid":"6109f3778036fc05f0498059"}},
{"_id":{"$oid":"60f7d1388036fc0fc04d1a11"}},
{"_id":{"$oid":"60f7d13c8036fc0fc04d1a13"}},
{"_id":{"$oid":"60ffcac28036fc05f0498021"}},
{"_id":{"$oid":"60ffcac68036fc05f0498023"}},
{"_id":{"$oid":"60efeabf8036fc062411c4e7"}},
{"_id":{"$oid":"60fa22a08036fc0fc04d1f60"}},
{"_id":{"$oid":"60f0eb098036fc062411c517"}},
{"_id":{"$oid":"60f912f98036fc0fc04d1c9b"}},
{"_id":{"$oid":"6109eef48036fc05f049803f"}},
{"_id":{"$oid":"610b3f3a8036fc05f0498074"}},
{"_id":{"$oid":"60f7864f8036fc1c30913dbd"}},
{"_id":{"$oid":"60f68c028036fc131c569198"}},
{"_id":{"$oid":"610b41288036fc05f0498079"}},
{"_id":{"$oid":"60f7864f8036fc1c30913dbd"}},
{"_id":{"$oid":"60fa22fc8036fc0fc04d1f6c"}},
{"_id":{"$oid":"610a5e408036fc05f049806f"}},
{"_id":{"$oid":"60f785e58036fc1c30913db1"}},
{"_id":{"$oid":"60f666da8036fc232cb5f2bb"}},
{"_id":{"$oid":"6109eef48036fc05f049803f"}},
{"_id":{"$oid":"60efeb708036fc062411c4ef"}},
{"_id":{"$oid":"60f785e58036fc1c30913db1"}},
{"_id":{"$oid":"60f635ca8036fc22b0f481f1"}},
{"_id":{"$oid":"60f635ca8036fc22b0f481f1"}},
{"_id":{"$oid":"60f635ca8036fc22b0f481f1"}},
{"_id":{"$oid":"60efc8268036fc062411c4b8"}},
{"_id":{"$oid":"60f635ca8036fc22b0f481f1"}},
{"_id":{"$oid":"60efeac28036fc062411c4e9"}}]}' --authenticationDatabase admin -d qdcares -c sfss -o /home/mongod/mongo/database/export/sfss.json
```