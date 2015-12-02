title: 安装MySql经GUI客户端MySql-Workbench
date: 2015-01-05 14:13:16
tags: Ubuntu
---
### 一.mysql安装  
``` bash
　　sudo apt-get install mysql-server mysql-client  
```
**(安装过程有窗口弹出询问 mysql root 用户密码)  **
#### 一旦安装完成，MySQL 服务器应该自动启动。您可以在终端提示符后运行以下命令来检查 MySQL 服务器是否正在运行
``` bash
$ sudo netstat -tapln | grep mysql  
tcp 0 0 0.0.0.0:3306 0.0.0.0:* LISTEN 1293/mysqld
```
#### 如果服务器不能正常运行，您可以通过下列命令启动它：
``` bash
　　sudo /etc/init.d/mysql restart
```
#### 让MySQL数据库能被远程访问  
　　在Ubuntu下MySQL缺省是只允许本地访问的，如果你要其他机器也能远程够访问这台Mysql数据库的话，需要设置一些东西，下面我们一步步地来：
``` bash
    $ sudo gedit /etc/mysql/my.cnf  
```
将bind-address=127.0.0.1改成 bind-address= 你机器的IP 这样就可以允许其他机器访问MySQ,或者直接注释掉也可

### 二.workbench
``` bash
sudo apt-get install mysql-workbench
```
#### 装完后，在菜单 应用程序 | 编程 下面会有 Mysql Workbench
