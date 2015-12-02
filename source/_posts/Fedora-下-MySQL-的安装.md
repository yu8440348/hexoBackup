title: Fedora 下 MySQL 的安装
tags:
  - linux
  - fedora
categories: []
date: 2015-01-27 11:50:00
---
### 安装数据库

```
yum -y install community-mysql-server
```

### 配置数据库

``` bash
#启动服务
systemctl start mysqld.service
systemctl enable mysqld.service
#以root登陆数据库
mysql -u root
#user列表
mysql> select user, host, password from mysql.user;
#删除空user
delete from mysql.user where user='';
#设置密码
set password for root@localhost=password('123456');
set password for root@rachel=password('123456');
set password for root@'127.0.0.1'=password('123456');
#退出
mysql> exit
```
