## Kibana安装

#### 1. 安装 Kibana

将下载下来的kibana-7.17.4-windows-x86_64.zip解压

#### 2. 修改文件

编辑`config\kibana.yml`文件，在文件末尾增加如下配置：

```yml
#IP访问地址和端口号
server.host: "localhost"
server.port: 5601

#为了避免访问 Kibana 出现server.publicBaseUrl 缺失,在生产环境中运行时应配置。某些功能可能运行不正常的提示，增加如下配置;
#localhost替换为对应ip，如部在192.16.1.151，则修改为 http://192.16.1.151:5601/
server.publicBaseUrl: "http://localhost:5601/"

#ElasticSearch连接地址
elasticsearch.hosts: ["http://192.16.1.151:9200"]

#设置访问用户
elasticsearch.username: "elastic"
#设置访问密码
elasticsearch.password: "qdcares805"

#设置中文显示
i18n.locale: "zh-CN"
```

#### 3. 启动Kibana

双击执行`bin`目录下`kibana.bat`；

