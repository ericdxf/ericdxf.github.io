# Nginx常用命令

### 启动命令

```
nginx -c /etc/nginx/conf/nginx.conf
```

### 关闭命令

```
ps -ef|grep nginx
kill -QUIT 2072
```

### 配置文件检测

```
nginx -t
```

### 重新加载配置文件

```
nginx -s reload
```

### nginx配置location [=|~|~*|^~] /uri/ { … }用法

- = 严格匹配。如果这个查询匹配，那么将停止搜索并立即处理此请求。

- ~ 为区分大小写匹配(可用正则表达式)

- !~为区分大小写不匹配

- ~* 为不区分大小写匹配(可用正则表达式)

- !~*为不区分大小写不匹配

- ^~ 如果把这个前缀用于一个常规字符串,那么告诉nginx 如果路径匹配那么不测试正则表达式。

  ###### 示例

location = / {

\# 只匹配 / 查询。

}

location / {

\# 匹配任何查询，因为所有请求都以 / 开头。但是正则表达式规则和长的块规则将被优先和查询匹配。

}

location ^~ /p_w_picpaths/ {

\# 匹配任何以 /p_w_picpaths/ 开头的任何查询并且停止搜索。任何正则表达式将不会被测试。

}

location ~*.(gif|jpg|jpeg)$ {

\# 匹配任何以 gif、jpg 或 jpeg 结尾的请求。

}

location ~*.(gif|jpg|swf)$ {

valid_referers none blocked start.igrow.cn sta.igrow.cn;

if ($invalid_referer) {

\#防盗链

rewrite ^/ http://$host/logo.png;

}

}





### proxy_pass

在nginx中配置proxy_pass代理转发时，如果在proxy_pass后面的url加/，表示绝对根路径；如果没有/，表示相对路径，把匹配的路径部分也给代理走。

假设下面四种情况分别用 http://192.168.1.1/proxy/test.html 进行访问。

##### 第一种：

location /proxy/ {
	proxy_pass http://127.0.0.1/;
}
代理到URL：http://127.0.0.1/test.html

##### 第二种（相对于第一种，最后少一个 / ）

location /proxy/ {
	proxy_pass http://127.0.0.1;
}
代理到URL：http://127.0.0.1/proxy/test.html

##### 第三种：

location /proxy/ {
	proxy_pass http://127.0.0.1/aaa/;
}
代理到URL：http://127.0.0.1/aaa/test.html

##### 第四种（相对于第三种，最后少一个 / ）

location /proxy/ {
	proxy_pass http://127.0.0.1/aaa;
}
代理到URL：http://127.0.0.1/aaatest.html



### rewrite 最后一项flag参数：

- last

1. 结束当前的请求处理，用替换后的URI重新匹配location；
2. 可理解为重写（rewrite）后，发起了一个新请求，进入server模块，匹配location；
3. 如果重新匹配循环的次数超过10次，nginx会返回500错误；
4. 返回302 http状态码 ；
5. 浏览器地址栏显示重地向后的url

- break

1. 结束当前的请求处理，使用当前资源，不在执行location里余下的语句；
2. 返回302 http状态码 ；
3. 浏览器地址栏显示重地向后的url

-  redirect

1. 临时跳转，返回302 http状态码；
2. 浏览器地址栏显示重地向后的url

- permanent

1. 永久跳转，返回301 http状态码；
2. 浏览器地址栏显示重定向后的url

### 示例

- 访问Url：   http://192.16.1.144:5556/api/lostFound-service/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:9999/travel-service/travel/lostFound/list


```nginx
location ^~ /api/lostFound-service {
    proxy_pass http://192.16.1.144:9999/travel-service;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic1/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:8767/travel/lostFound/list

```nginx
location ^~ /api/lostFound-servic1/ {
    proxy_pass http://192.16.1.144:8767/;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic2/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:8767/api/lostFound-servic2/travel/lostFound/list (可以发现这种方式只修改location匹配字段前面的信息)

```nginx
location ^~ /api/lostFound-servic2/ {
    proxy_pass http://192.16.1.144:8767;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic3/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:8767/travel/lostFound/list

```nginx
location ^~ /api/lostFound-servic3 {
    rewrite /api/lostFound-servic3/(.*) /$1 break;
    proxy_pass http://192.16.1.144:8767;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic4/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:9999/travel-service/travel/lostFound/list

```nginx
location ^~ /api/lostFound-servic4/ {
    proxy_pass http://192.16.1.144:9999/travel-service/;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic5/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:9999/travel-service/travel/lostFound/list
- 类比上一条可以发现：location和proxy_pass结尾都写`/`等价于都不写

```nginx
location ^~ /api/lostFound-servic5 {
    proxy_pass http://192.16.1.144:9999/travel-service;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

- 访问Url：   http://192.16.1.144:5556/api/lostFound-servic6/travel/lostFound/list
- 反向代理结果：http://192.16.1.144:9999/travel-service/travel/lostFound/list

```nginx
location ^~ /api/lostFound-servic6 {
    rewrite /api/lostFound-servic6/(.*) /travel-service/$1 break;
    proxy_pass http://192.16.1.144:9999;
    proxy_redirect off;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 180s;
    #proxy_set_header Host $proxy_host;
}
```

### 缓存配置

频繁请求时，需添加缓存解决nginx 502错误，或者header过大也会导致502错误，需要添加缓存解决

```nginx
location ^~ /jkqd/ {
    resolver 114.114.114.114  valid=10s;
    resolver_timeout 3s;

    rewrite /jkqd/(.*) /api/$1 break;
    proxy_pass http://$jkqdnginx;
    proxy_redirect off;
    proxy_set_header Host $proxy_host;
    proxy_set_header Connection "Keep-Alive";			
    proxy_connect_timeout 30s;
    proxy_read_timeout 600s;
    proxy_send_timeout 100s;
    # 缓存配置
    proxy_buffering on;
    proxy_buffer_size 100k;
    proxy_buffers 32 256k;
    proxy_busy_buffers_size 512k;
    proxy_temp_file_write_size 256k;
}
```

