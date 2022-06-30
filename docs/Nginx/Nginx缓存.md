### Nginx缓存

#### proxy_cache_path

**语法**：

```
proxy_cache_path path [levels=levels] keys_zone=name:size [inactive=time] [max_size=size] [loader_files=number] [loader_sleep=time] [loader_threshold=time];
默认值:    —
上下文:    http
```

**示例**：

```
proxy_cache_path /cache levels=1:2 keys_zone=cache:10m max_size=10g inactive=60m use_temp_path=off;
```

- `path` 指定缓存文件目录，和 proxy_temp_path 最好设置在同一文件分区下，缓存内容是先写在 temp_path，临时文件和缓存可以放在不同的文件系统，将导致文件在这两个文件系统中进行拷贝，而不是廉价的重命名操作。因此，针对任何路径，都建议将缓存和proxy_temp_path指令设置的临时文件目录放在同一文件系统。
- `level` 定义了缓存的层次结构，每层可以用1（最多16中选择，0-f）或2（最多256种选择，00-ff）表示，中间用 [冒号] 分隔。“levels=1:2”表示开启1、2层级(第2层级理论有16*256个目录)。

```haskell
proxy_cache_path /data/nginx/cache;  # 所有缓存只有一个目录
/data/nginx/cache/d7b6e5978e3f042f52e875005925e51b 

proxy_cache_path /data/nginx/cache levels=1:2;  # 第二层级有16*256=4096个目录
/data/nginx/cache/b/51/d7b6e5978e3f042f52e875005925e51b 

proxy_cache_path /data/nginx/cache levels=1:1:1; #第三层级有16*16*16个目录
/data/nginx/cache/b/1/5/d7b6e5978e3f042f52e87500592 

proxy_cache_path /data/nginx/cache levels=2; # 第一层级有256个目录
/data/nginx/cache/1b/d7b6e5978e3f042f52e875005925e51b 
```

- `keys_zone` 指定一个共享内存空间zone，所有活动的键和缓存数据相关的信息都被存放在共享内存中，这样nginx可以快速判断一个request是否命中或者未命中缓存，1m可以存储8000个key，10m可以存储80000个key；
- `inactive` inactive=30m 表示 30 分钟没有被访问的文件会被 cache manager 删除，inactive的默认值是10分钟。 需要注意的是，inactive和expired配置项的含义是不同的，expired只是缓存过期，但不会被删除，inactive是删除指定时间内未被访问的缓存文件
- `max_size` cache存储的最大尺寸，如果不指定，会用掉所有磁盘空间，当尺寸超过，将会基于LRU算法移除数据，以减少占用大小。nginx启动时，会创建一个“Cache manager”进程，通过“purge”方式移除数据。
- `loader_files` “cache loader”进程遍历文件时，每次加载的文件个数。默认为100.
- `loader_threshold` 每次遍历消耗时间上限。默认为200毫秒。
- `loader_sleep` 一次遍历之后，停顿的时间间隔，默认为50毫秒。

#### proxy_cache

**语法**：

```css
proxy_cache zone | off;
默认值:    proxy_cache off;
上下文:    http, server, location
```

指定用于页面缓存的共享内存。同一块共享内存可以在多个地方使用。off参数可以屏蔽从上层配置继承的缓存功能。zone名称由`proxy_cache_path`指令定义。

#### proxy_cache_key

**语法**：

```puppet
proxy_cache_key string;
默认值:    
proxy_cache_key $scheme$proxy_host$request_uri;
上下文:    http, server, location
```

定义如何生成缓存的键，比如

```kotlin
proxy_cache_key "$host$request_uri $cookie_user";
```

这条指令的默认值类似于下面字符串

```puppet
proxy_cache_key $scheme$proxy_host$uri$is_args$args;
```

缓存文件并不是越多越好，所以 cache_key 的设计非常关键。代理或 URL 跳转常常会添加的无用请求参数，这就会出现不同的 cache_key 保存了多份相同的缓存内容，这对缓存效果影响很大。通过 ngx_lua 可以对 URL 参数进行过滤，保证 cache_key 唯一。

#### proxy_cache_valid

**语法**：

```css
proxy_cache_valid [code ...] time;
默认值:    —
上下文:    http, server, location
```

为不同的响应状态码设置不同的缓存时间。比如，下面指令

```undefined
proxy_cache_valid 200 302 10m;
proxy_cache_valid 404      1m;
```

设置状态码为200和302的响应的缓存时间为10分钟，状态码为404的响应的缓存时间为1分钟。

如果仅仅指定了time，

```undefined
proxy_cache_valid 5m;
```

那么只有状态码为200、300和302的响应会被缓存。

如果使用了any参数，那么就可以缓存任何响应：

```r
proxy_cache_valid 200 302 10m;
proxy_cache_valid 301      1h;
proxy_cache_valid any      1m;
```

缓存参数也可以直接在响应头中设定。这种方式的优先级高于使用这条指令设置缓存时间。

> “X-Accel-Expires”响应头可以以秒为单位设置响应的缓存时间，如果值为0，表示禁止缓存响应，如果值以@开始，表示自1970年1月1日以来的秒数，响应一直会被缓存到这个绝对时间点。
>
> 如果不含“X-Accel-Expires”响应头，缓存参数仍可能被“Expires”或者“Cache-Control”响应头设置。
>
> 如果响应头含有“Set-Cookie”，响应将不能被缓存。 这些头的处理过程可以使用指令proxy_ignore_headers忽略。

#### proxy_ignore_headers

**语法**：

```css
proxy_ignore_headers field ...;
默认值:    —
上下文:    http, server, location
```

指定来自后端server的响应中的某些header不会被处理，如下几个fields可以被ignore：“X-Accel-Redirect”、“X-Accel-Expires”、“X-Accel-Limit-Rate”、“X-Accel-Buffering”、“X-Accel-Charset”、“Expires”、“Cache-Control”、“Set-Cookie”、“Vary”。“不被处理”就是nginx不会尝试解析这些header并应用它们，比如nginx处理来自后端server的“Expires”，将会影响它本地的文件cache的机制

如果不被取消，这些头部的处理可能产生下面结果：

> “X-Accel-Expires”，“Expires”，“Cache-Control”，和“Set-Cookie” 设置响应缓存的参数；
>
> “X-Accel-Redirect”执行到指定URI的内部跳转；
>
> “X-Accel-Limit-Rate”设置响应到客户端的传输速率限制；
>
> “X-Accel-Buffering”启动或者关闭响应缓冲；
>
> “X-Accel-Charset”设置响应所需的字符集。

#### proxy_hide_header

**语法**：

```css
proxy_hide_header field;
默认值:    —
上下文:    http, server, location
```

nginx默认不会将“Date”、“Server”、“X-Pad”，和“X-Accel-...”响应头发送给客户端。proxy_hide_header指令则可以设置额外的响应头，这些响应头也不会发送给客户端。相反的，如果希望允许传递某些响应头给客户端，可以使用proxy_pass_header指令。

#### proxy_pass_header

**语法**：

```css
proxy_pass_header field;
默认值:    —
上下文:    http, server, location
```

允许传送被屏蔽的后端服务器响应头到客户端。

#### proxy_cache_min_uses

```css
proxy_cache_min_uses number;
默认值:    proxy_cache_min_uses 1;
上下文:    http, server, location
```

设置响应被缓存的最小请求次数。默认为1，当客户端发送相同请求达到规定次数后，nginx才对响应数据进行缓存；指定请求至少被发送了多少次以上时才缓存，可以防止低频请求被缓存

#### proxy_cache_use_stale

**语法**：

```vbnet
proxy_cache_use_stale error | timeout | invalid_header | updating | http_500 | http_502 | http_503 | http_504 | http_404 | off ...;
默认值:    proxy_cache_use_stale off;
上下文:    http, server, location
```

如果后端服务器出现状况，nginx是可以使用过期的响应缓存的。这条指令就是定义何种条件下允许开启此机制。这条指令的参数与proxy_next_upstream指令的参数相同。

此外，updating参数允许nginx在正在更新缓存的情况下使用过期的缓存作为响应。这样做可以使更新缓存数据时，访问源服务器的次数最少。

在植入新的缓存条目时，如果想使访问源服务器的次数最少，可以使用proxy_cache_lock指令。



#### 参考地址

https://blog.csdn.net/weixin_30795127/article/details/97385091#proxy_cache_valid