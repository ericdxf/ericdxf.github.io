#### IP、TCP头部长度20字节

#### 以太网帧头帧尾共18字节

#### IPv6首部长度40字节

1. 版本：4位，对于IPv6，本字段值必须为6
2. 通信量类：8位，区分不同IPv6数据报的类别或优先级
3. 流标号：20位
4. 有效径荷长度：16位
5. 下一个首部：8位，指出了IPv6头后所跟的头字段中的协议类型
6. 跳数限制：8位，每转发一次改值减一，到0则丢弃，用于高层设置其超时值
7. 源地址：128位，发送方地址
8. 目的地址：128位，接收方地址

#### 端口记录

```
http：80
https:443
tomcat：8080
  
ftp：21
ssh：22
telnet:23
smtp：25
pop3:110（邮件接收端口）
  
php-fpm：9000
```

####  千兆以太网光纤传输介质

![image-20211027130915526](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20211027130915526.png)

#### AES加密

![image-20211027132115901](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20211027132115901.png)

![image-20211027132318251](https://bucket-1312501492.cos.ap-nanjing.myqcloud.com/img/image-20211027132318251.png)

