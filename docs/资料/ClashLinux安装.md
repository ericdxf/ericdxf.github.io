## ClashLinux安装

### 下载

[github.com/Dreamacro/clash/releases](https://github.com/Dreamacro/clash/releases)

选择最新版本的 clash-linux-amd64 安装包。

### 上传

1. 在 `/etc`目录下创建 `clash`目录；
2. 运行 `cd /etc/clash`切换目录，运行 `rz`命令上传下载的 clash-linux-amd64 安装包；
3. 运行 `gzip -d clash-linux-amd64`解压安装包；
4. 运行 `mv clash-linux-amd64 clash`修改运行文件名；
5. 拷贝自己的`clash config.yaml`到`/etc/clash`及`/root/.config/clash`目录下；

### 测试运行

1. 运行 `chmod +x clash`添加权限；
2. 执行`./clash`启动会自动下载`Country.mmdb`文件，该文件为全球IP库，可以实现各个国家的IP信息解析和地理定位，如果下载不下来请自己网上找资源下载，资源很多；
3. `Country.mmdb`文件也要保证`/etc/clash`及`/root/.config/clash`两个目录都有；

会下载 Country.mmdb，需要等待一会。

1. 运行 `export https_proxy=http://127.0.0.1:7890`修改系统代理；

临时系统代理，退出终端会失效。

### 守护进程

1. 运行 `cp clash /usr/local/bin`；
2. 创建 `/etc/systemd/system/clash.service`；

```ini
[Unit]
Description=Clash daemon, A rule-based proxy in Go.
After=network.target

[Service]
Type=simple
Restart=always
ExecStart=/usr/local/bin/clash -d /etc/clash

[Install]
WantedBy=multi-user.target
复制代码
```

1. 运行 `systemctl enable clash`设置 clash 服务在系统启动时运行；
2. 运行 `systemctl start clash`立即运行 clash 服务；
3. 运行 `systemctl status clash`查看 clash 服务运行状态；

![img](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/a4d3e825a23f4319bf8f9c5eba20bbfb~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

1. 运行 `journalctl -xe`查看运行日志；

### 系统代理

1. 运行 `cd ~`切换到 root 账户目录；
2. 运行 `vim /etc/profile`编辑，添加系统代理；

```shell
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
export all_proxy=socks5://127.0.0.1:7890
```

最后执行`source /etc/profile`命令刷新配置

运行 `curl https://www.google.com`测试；

