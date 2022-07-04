mysql数据库时间和本地时间相差13个小时，原因是mysql默认使用的是CST市区，这个是美国中部时间，和北京时间刚好相差13个小时  
修改方法就是在`/etc/my.cnf`文件中添加一行代码即可。

```
default-time-zone = '+08:00' 
```
添加完成记得重启下mysql服务
```
[root@VM_94_149_centos home]# service mysqld restart
```
