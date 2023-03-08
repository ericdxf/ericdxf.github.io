### Agent的可配置属性列表

| 属性名                                                       | 描述                                                         | 默认值               |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| agent.namespace                                              | 命名空间，用于隔离跨进程传播的header。如果进行了配置，header将为HeaderName:Namespace. | 未设置               |
| agent.service_name                                           | 在SkyWalking UI中展示的服务名。5.x版本对应Application，6.x版本对应Service。 建议：为每个服务设置个唯一的名字，服务的多个服务实例为同样的服务名 | Your_ApplicationName |
| agent.sample_n_per_3_secs                                    | 负数或0表示不采样，默认不采样。SAMPLE_N_PER_3_SECS表示每3秒采样N条。 | 未设置               |
| agent.authentication                                         | 鉴权是否开启取决于后端的配置，可查看application.yml的详细描述。对于大多数的场景，需要后端对鉴权进行扩展。目前仅实现了基本的鉴权功能。 | 未设置               |
| agent.span_limit_per_segment                                 | 单个segment中的span的最大个数。通过这个配置项，Skywalking可评估应用程序内存使用量。 | 未设置               |
| agent.ignore_suffix                                          | 如果这个集合中包含了第一个span的操作名，这个segment将会被忽略掉。 | 未设置               |
| agent.is_open_debugging_class                                | 如果为true，skywalking会将所有经Instrument转换过的类文件保存到/debugging文件夹下。Skywalking团队会要求你提供这些类文件以解决兼容性问题。 | 未设置               |
| agent.cause_exception_depth                                  | 在记录异常信息的时候, 探针需要记录的堆栈深度.                | 5                    |
| agent.force_reconnection_period                              | grpc的强制重连周期，基于grpc_channel_check_interval.         | 1                    |
| agent.operation_name_threshold                               | 设置操作名不建议超过最大长度(190).                           | 150                  |
| agent.keep_tracing                                           | 如果该值为 true，即使后台不可用，也要保持跟踪.               | FALSE                |
| osinfo.ipv4_list_size                                        | 限制ipv4列表的长度.                                          | 10                   |
| collector.backend_service                                    | 接收skywalking trace数据的后端地址                           | 127.0.0.1:11800      |
| collector.heartbeat_period                                   | 探针心跳报告时间. 单位秒.                                    | 30                   |
| collector.grpc_upstream_timeout                              | grpc客户端向上游发送数据时超时多长时间. 单位秒.              | 30 秒                |
| collector.get_profile_task_interval                          | 嗅探器获取配置文件任务列表间隔.                              | 20                   |
| logging.level                                                | 日志级别。默认为debug。                                      | DEBUG                |
| logging.file_name                                            | 日志文件名                                                   | skywalking-api.log   |
| logging.output                                               | 日志输出. 默认是文件. 使用控制台意味着输出到标准输出.        | FILE                 |
| logging.dir                                                  | 日志目录。默认为空串，表示使用"system.out"输出日志。         |                      |
| logging.pattern                                              | 日志格式. 所有的转换说明符:<br/>  * %level means log level.<br/>  * %timestamp 表示现在的时间格式 yyyy-MM-dd HH:mm:ss:SSS.<br/>  * %thread 表示当前线程的名称.<br/>  * %msg 表示用户记录的某些消息.<br/>  * %class 表示TargetClass的SimpleName.<br/>  * %throwable 表示用户抛出的异常.<br/>  * %agent_name 表示 agent.service_name |                      |
| logging.max_file_size                                        | 日志文件的最大大小。当日志文件大小超过这个数，归档当前的日志文件，将日志写入到新文件。 | 300 * 1024 * 1024    |
| logging.max_history_files                                    | The max history log files. When rollover happened, if log files exceed this number,then the oldest file will be delete. Negative or zero means off, by default. | -1                   |
| jvm.buffer_size                                              | 收集JVM信息的buffer的大小。                                  | 60 * 10              |
| buffer.channel_size                                          | buffer的channel大小。                                        | 5                    |
| buffer.buffer_size                                           | buffer的大小                                                 | 300                  |
| profile.active                                               | 如果为true，SkyWalking代理将在用户创建新的配置文件任务时启用配置文件. 否则禁用概要. | TRUE                 |
| profile.max_parallel                                         | 并行监控段计数                                               | 5                    |
| profile.duration                                             | 监控段最大时间(分钟)，如果当前监控段时间超出限制，则停止.    | 10                   |
| profile.dump_max_stack_depth                                 | 最大线程转储的堆栈深度                                       | 500                  |
| profile.snapshot_transport_buffer_size                       | 快照传输到后端缓冲区的大小                                   | 50                   |
| plugin.mongodb.trace_param                                   | 如果为true，记录所有访问MongoDB的参数信息。默认为false，表示仅记录操作名，不记录参数信息。 | FALSE                |
| plugin.elasticsearch.trace_dsl                               | 如果为true，记录所有访问ElasticSearch的DSL信息。默认为false。 | FALSE                |
| plugin.springmvc.use_qualified_name_as_endpoint_name         | 如果为true，endpoint的name为方法的全限定名，而不是请求的URL。默认为false。 | FALSE                |
| plugin.toolit.use_qualified_name_as_operation_name           | 如果为true，operation的name为方法的全限定名，而不是给定的operation name。默认为false。 | FALSE                |
| plugin.mysql.trace_sql_parameters                            | 如果设置为 true, SQL 查询 (典型的是 java.sql.PreparedStatement) 的参数也会被采集. | FALSE                |
| plugin.mysql.sql_parameters_max_length                       | 如果设置为正整数, 收集的 SQL 参数 db.sql.parameters 会被截断到这个长度, 否则会被完整保存, 这可能会导致性能问题. | 512                  |
| plugin.solrj.trace_statement                                 | 如果为 true, 追踪 Solr 查询请求中的所有查询参数(包括 deleteByIds 和 deleteByQuery) 默认为 false. | FALSE                |
| plugin.solrj.trace_ops_params                                | 如果为 true, 追踪 Solr 查询中所有操作参数, 默认为 false.     | FALSE                |
| plugin.peer_max_length                                       | Peer 描述最大限制.                                           | 200                  |
| plugin.mongodb.filter_length_limit                           | 如果设置为正数, WriteRequest.params 将被截短到这个长度, 否则它将被完全保存，这可能会导致性能问题. | 256                  |
| plugin.postgresql.trace_sql_parameters                       | 如果设置为true，将收集sql的参数(通常是 java.sql.PreparedStatement). | FALSE                |
| plugin.postgresql.sql_parameters_max_length                  | 如果设置为正数, db.sql.parameters 将被截断到这个长度，否则它将被完全保存，这可能会导致性能问题. | 512                  |
| plugin.mariadb.trace_sql_parameters                          | 如果设置为true，将收集sql的参数(通常是 java.sql.PreparedStatement). | FALSE                |
| plugin.mariadb.sql_parameters_max_length                     | 如果设置为正数，db.sql 将被截断到这个长度，否则它将被完全保存，这可能会导致性能问题. | 512                  |
| plugin.light4j.trace_handler_chain                           | 如果为true，请跟踪属于请求的Light4J处理程序链的所有中间件/业务处理程序. | FALSE                |
| plugin.opgroup.*                                             | 支持在不同插件中自定义组规则的操作名. 请阅读 Group rule supported plugins | Not set              |
| plugin.springtransaction.simplify_transaction_definition_name | 如果为true，事务定义名称将被简化.                            | FALSE                |
| plugin.jdkthreading.threading_class_prefixes                 | 线程类 (java.lang.Runnable and java.util.concurrent.Callable) 及其子类(包括任何名称匹配 THREADING_CLASS_PREFIXES (以 ， 分隔)的匿名内部类) 将被检测, 确保只指定短小的前缀，就像您预期要测试的一样, (java. 和 javax. 会因安全问题而被忽略) | Not set              |
| plugin.tomcat.collect_http_params                            | 这个配置项控制Tomcat插件是否应该收集请求的参数. 同样，在概要追踪中隐式激活. | FALSE                |
| plugin.springmvc.collect_http_params                         | 这个配置项控制SpringMVC插件是否应该收集请求的参数, 当您的Spring应用程序基于Tomcat时, 只需要设置 plugin.tomcat.collect_http_params 或 plugin.springmvc.collect_http_params 之一. 同样，在概要追踪中隐式激活. | FALSE                |
| plugin.http.http_params_length_threshold                     | 当启用 COLLECT_HTTP_PARAMS时，要保留多少字符并将其发送到OAP后端，请使用负值来保留和发送完整的参数. 添加这个配置项是为了提高性能. | 1024                 |
| correlation.element_max_number                               | 关联上下文的最大元素数.                                      | 3                    |
| correlation.value_max_length                                 | 关联上下文元素的最大值长度.                                  | 128                  |