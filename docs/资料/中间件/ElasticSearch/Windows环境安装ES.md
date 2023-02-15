## Windows环境安装ES

#### 1. 下载安装包

当前使用版本为7.15.2

下载地址：

https://www.elastic.co/downloads/past-releases/elasticsearch-7-15-2

#### 2. 安装 ElasticSearch

将`elasticsearch-7.15.2-windows-x86_64.zip` 解压到`elasticsearch-7.15.2`目录下。

#### 3. 修改环境变量

因为新版的 ElasticSearch 已经弃用了 JAVA_HOME 环境变量，转而使用了 ES_JAVA_HOME 环境变量，并且在新版的安装包中已经提供了 Java11 运行环境，因此我们需要增加 ES_JAVA_HOME 这个环境变量，不然在后续配置中可能会出现“warning: usage of JAVA_HOME is deprecated, use ES_JAVA_HOME”的警告信息。

我们在系统变量中新建变量名为`ES_JAVA_HOME`，值为安装目录下jdk目录`elasticsearch-7.15.2\jdk`的环境变量，如下图所示：

![image-20230214154900989](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230214154900989.png)

#### 4. 修改 elasticsearch.yml 文件

编辑`config\elasticsearch.yml`文件，在文件末尾增加如下配置：

```yml
#设置快照存储地址(可不设置)
path.repo: ["D:\\Net_Program\\Net_ElasticSearch\\backup"]

#数据存放路径(可不设置，默认就是如下地址)
path.data: G:/elasticsearch-7.15.2/datas
#日志存放路径(可不设置，默认就是如下地址)
path.logs: G:/elasticsearch-7.15.2/logs

#节点名称
node.name: node-1
#节点列表(设置为当前服务器ip即可)
discovery.seed_hosts: ["192.168.3.200"]
#初始化时master节点的选举列表
cluster.initial_master_nodes: ["node-1"]

#集群名称(单机可不设置)
cluster.name: es-main
#对外提供服务的端口
http.port: 9200
#内部服务端口
transport.port: 9300

#启动地址，如果不配置，只能本地访问，如下配置则不限制访问ip地址
network.host: 0.0.0.0
#跨域支持
http.cors.enabled: true
#跨域访问允许的域名地址
http.cors.allow-origin: "*"
```

#### 5. 修改 JVM 内存

如果你的服务器内存有限，则需要根据实际情况设置 ElasticSearch 的内存限制。

编辑 `config` 文件夹中的`jvm.options`文件，增加如下配置即可，此处我设置的是 4G 范围内。

特别说明：此步骤需要注意，建议最好根据服务器的内存情况进行设置，以免后期再来调整。同时此步骤需要在将 ElasticSearch 安装为服务前进行设置，否则安装服务后，即便是重启服务也不会生效。

```yml
#设置最小内存
-Xms4g
#设置最大内存
-Xmx4g
```

>
> 注意
>

> 我们在设置-Xms和-Xmx属性的时候，一定要设置为相同的值，否则在启动服务的时候出现如下的错误，倒是启动 ElasticSearch 服务失败。
>

比如我们都可以设置为 4g，-Xms4g 和 -Xmx4g

#### 6. 设置访问密码

启用访问账户密码限制，编辑`config\elasticsearch.yml`文件，在文件末尾增加如下配置：

```yml
xpack.security.enabled: true
xpack.license.self_generated.type: basic
xpack.security.transport.ssl.enabled: true
```

1. 重启es；

2. 进入bin目录，输入`elasticsearch-setup-passwords interactive`初始化密码；

3. 因为需要设置 elastic，apm_system，kibana，kibana_system，logstash_system，beats_system，remote_monitoring_user 这些用户的密码，故这个过程比较漫长，耐心设置；

4. 访问`localhost:9200` 地址测试账号密码是否配置成功；

   ![ElasticSearch之Windows中环境安装_搜索引擎_09](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/30180327_635e4befa693910180.png)

#### 7. 安装 ElasticSearch 服务

以管理员身份运行 CMD 并定位到 ElasticSearch 的 bin 目录，执行如下命令：

```shell
elasticsearch-service.bat install
```

![image-20230214161021426](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230214161021426.png)

执行完后 Windows 服务中就多了一个显示名称为 Elasticsearch 7.15.2 (elasticsearch-service-x64)的服务，如下图所示：

![image-20230214161226256](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20230214161226256.png)

#### 8. 启动服务

- 服务启动：右键服务名称后启动；

- 脚本启动：双击执行`bin`目录下`elasticsearch.bat`；

**注意：9300是tcp通信端口，es集群之间使用tcp进行通信，9200是http协议端口。**

