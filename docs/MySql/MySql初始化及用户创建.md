CentOS安装MySql8.0之后，部分配置和之前的5.7有比较大的差异，在这里整理一下  
使用的安装方式是rpm安装

```bash
#wget http://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm  
rpm -ivh mysql80-community-release-el7-1.noarch.rpm  
```
然后执行安装命令
```bash
yum -y install mysql mysql-server mysql-devel  
```
再次执行安装命令可查看安装情况，会给出提示，表示安装成功
```bash
[root@VM_94_149_centos www]# yum -y install mysql mysql-server mysql-devel
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
Package mysql-community-client-8.0.12-1.el7.x86_64 already installed and latest version
Package mysql-community-server-8.0.12-1.el7.x86_64 already installed and latest version
Package mysql-community-devel-8.0.12-1.el7.x86_64 already installed and latest version
Nothing to do
```
其他一些相关指令记录如下  
查看默认密码
```sql
mysql> grep "A temporary password" /var/log/mysqld.log
```
修改默认密码
```sql
mysql> ALTER USER USER() IDENTIFIED BY 'newPassword';
```
创建用户
```sql
mysql> create user eric@'%' identified by 'your password';
```
由于机密方式不同导致连接不上的问题处理
```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'your password' PASSWORD EXPIRE NEVER;
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your password';
mysql> flush privileges;
```
授权，默认创建的用户权限是usage,就是无权限，只能登录而已，（all：所有权限，这里有select,update等等权限，可以去搜一下；后面的*.*：指定数据库.指定表，这里是所有；to后面就是你刚才创建的用户）
```sql
mysql> grant all on *.* to 'eric'@'%';
```
需要注意的一点是，8.0以后的MySql版本里密码验证是很严格的，必须要有大小写字母和特殊字符，上面的your password替换时要注意下。