title: dzen部署手册
tags:
 - Java开发
categories: []
date: 2016-08-17 14:38:00
---

### 部署activemq
下载apache-activemq-5.13.3部署到服务器中
### 部署dzen后台程序
#### 复制监控文件
> 将libsigar-amd64-linux.so文件复制到系统的 /usr/lib目录中，此文件主要用于调用系统的硬件信息，实现系统监控。

> 若是Windows系统将sigar-amd64-winnt.dll复制到JDK安装目录的bin目录中。 

#### 将dzen后台相关文件上传到服务器中，文件列表如下

```
lib --此目录主要包含dzen依赖的jar文件
dzen.jar --dzen后台程序入口
drivers.json --插件驱动配置文件，一般不需要修改
dzen.properties --主要配置文件
log4j.properties --日志配置文件
dzen.sh --dzen启动脚本文件
```
#### 修改dzen.properties配置文件
 
```
# file,database file代表本地模式从本地加载作业配置 database代表数据库模式，从数据库加载配置文件
config.source=database
# 数据库配置
config.db.type=mysql
config.db.username=root
config.db.password=123456
config.db.jdbcUrl=jdbc:mysql://10.177.129.141:3306/dzen?useUnicode=true&characterEncoding=utf8

#serverip，本地的IP地址用于web端运行监控显示
server.ip=10.44.55.32

# activemq broker config 消息服务IP地址
mq.service.broker.url=tcp://127.0.0.1:61616
mq.service.thread.count=3
mq.service.retry.times=3
# scheduler config
scheduler.job.thread.count=20

# datasource config
datasource.maxActive=20
datasource.maxIdle=20
datasource.minIdle=0
datasource.initialSize=30
datasource.maxWait=-1
datasource.defaultQueryTimeout=172800
datasource.removeAbandonedOnMaintenance=false
datasource.removeAbandonedOnBorrow=false
#datasource.removeAbandonedTimeout=300
#datasource.logAbandoned=true
datasource.testWhileIdle=true
datasource.testOnBorrow=true
datasource.testOnReturn=true
```
#### 运行dzen.sh 启动服务

```
#启动
sh dzen.sh start
#重启
sh dzen.sh restart
#停止
sh dzen.sh stop
```

### DZENweb端程序部署
> 将war包上传到tomcat中，修改以下配置文件

**config.properties配置文件中的BROKER_URL**

```
#服务器的消息服务地址
BROKER_URL=tcp://127.0.0.1:61616
```
**修改jdbc.properties中的数据库连接**

```
#database settings
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://192.168.1.251:3306/dzen?useUnicode=true&characterEncoding=utf8
jdbc.username=
jdbc.password=

#hibernate settings
hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
hibernate.show_sql=false
hibernate.format_sql=true
hibernate.hbm2ddl.auto=none
```
#### 启动tomcat