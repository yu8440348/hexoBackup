title: hive与mysql的安装配置
tags:
  - hive
categories: []
date: 2015-01-22 17:00:00
---
## hive的安装和配置
>java版本：jdk1.7
  hadoop版本：2.6.0
  Hive版本：0.9.0

<!--more-->
### 下载Hive
下载地址 http://mirrors.cnnic.cn/apache/hive/stable/  
我下载的是0.14稳定版，下载mysql JDBC驱动配置mysql要用到

### 把Hive移动到hadoop目录下并解压
```
mv hive-0.14.0.tar.gz /home/hadoop/
cd /home/hadoop/
tar -zxvf hive-0.14.0.tar.gz
```
### 用root用户给hive-0.9.0授权
```
su -
密码：
cd /home/hadoop/
sudo chown -R hadoop:hadoop hive-0.14.0
```
### 添加hive环境变量
```
vim /etc/profile
```
添加如下内容
```
# hive
export HIVE_HOME=/home/yuhaitao/hadoop/hive
export PATH=$HIVE_HOME/bin:${JAVA_HOME}/bin:$HADOOP_HOME/bin:$PATH
```
### 配置 Hive 配置文件
1.编辑 hive/conf/hive-env.sh
```
export HADOOP_HEAPSIZE=1024
HADOOP_HOME=/home/yuhaitao/hadoop
export HIVE_CONF_DIR=/home/yuhaitao/hadoop/hive/conf
```
2.配置 hive-default.xml 和 hive-site.xml
> 在“/home/hadoop/hive-0.9.0/conf”目录下,没有这两个文件,只有一个“hive-default.xml.template”,所以我们要复制两个“hive-default.xml.template”,并分别命名为“hive-default.xml”和“hive-site.xml” 如果当前是 root 用户,要把两个的文件的授权给 hadoop 用户。
```
cp hive-default.xml.template hive-default.xml
chown -R hadoop:hadoop hive-default.xml
cp hive-default.xml.template hive-site.xml
chown -R hadoop:hadoop hive-site.xml
ls -l
```
>  “hive-default.xml”用于保留默认配置,“hive-site.xml”用于个性化配置,可覆盖默认配置。

要连接mysql数据库作为数据源，hive-site.xml配置如下
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
<!--这四个参数一定要配置，否则报错-->
<property>  
<name>hive.exec.local.scratchdir</name>
<value>/home/yuhaitao/hive/temp</value>
</property>
<property>
<name>hive.downloaded.resources.dir</name>
<value>/home/yuhaitao/hive/temp</value>
</property>
<property>
 <name>hive.querylog.location</name>
<value>/home/yuhaitao/hive/temp</value>
</property>
<property>
<name>hive.server2.logging.operation.log.location</name>
<value>/home/yuhaitao/hive/operation_logs</value>
</property>
<!--该参数是设置HDFS中保存数据仓库的目录-->
<property>
<name>hive.metastore.warehouse.dir</name>
<value>/user/hive/warehouse</value>
</property>
<!--配置mysql数据库连接，-->
<property>
<name>hive.metastore.local</name>
<value>true</value>
</property>
 <property>
   <name>javax.jdo.option.ConnectionURL </name>
   <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
</property>
<property>
   <name>javax.jdo.option.ConnectionDriverName </name>
   <value>com.mysql.jdbc.Driver</value>
</property>
<property>
<name>javax.jdo.option.ConnectionUserName</name>
<value>root</value>
</property>
<property>
   <name>javax.jdo.option.ConnectionPassword</name>
   <value>123456</value>
</property>
</configuration>
```
### 把MySQL的JDBC驱动包复制到Hive的lib目录下。
### 验证Hive配置是否有误
```bash
yuhaitao@yuhaitao:~$ hive
15/01/22 17:26:52 WARN conf.HiveConf: DEPRECATED: Configuration property hive.metastore.local no longer has any effect. Make sure to provide a valid value for hive.metastore.uris if you are connecting to a remote metastore.
15/01/22 17:26:52 WARN conf.HiveConf: HiveConf of name hive.metastore.local does not exist

Logging initialized using configuration in file:/home/yuhaitao/hadoop/hive/conf/hive-log4j.properties
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/home/yuhaitao/hadoop/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/yuhaitao/hadoop/hive/lib/hive-jdbc-0.14.0-standalone.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
```
### 配置hwi web管理页面
下载apache-hive-0.14.0-src.tar.gz的源码包并解压  
解压后进入hwi子目录，该目录下的web文件夹正是war包的全部文件，我们需要做的就是通过jar命令把这个文件夹打包为war文件。  
在hwi目录下执行
```bash
jar cvfM0 hive-hwi-0.14.0.war -C web/ .
```
执行之后 ，将生成的war文件拷贝至$HIVE_HOME/lib文件夹下，同时修改hive-site.xml文件夹中的：  hive.hwi.war.file，将其value改为lib/hive-hwi-0.14.0.war

<property>
                 <name>hive.hwi.war.file</name>
                 <value>lib/hive-hwi-0.14.0.war</value>
</property>
再执行  
```
hive  --service hwi
```
打开浏览器输入   localhost:9999/hwi  成功打开页面
 如果出现以下错误：
> Unable to find a javac compiler;
com.sun.tools.javac.Main is not on the classpath.
Perhaps JAVA_HOME does not point to the JDK.
It is currently set to "/usr/lib/jdk/jre"

解决方法：
```
cp $JAVA_HOME/lib/tools.jar $HIVE_HOME/lib/
```

### Hive的数据类型

- 基本数据类型
- tinyint/smallint/int/bigint
- float/double
- boolean
- string
- 复杂数据类型
- Array/Map/Struct
- 没有date/datetime

### Hive常规操作
```bash
# 创建一个 test  数据库
hive > create  database  test ;
# 使用  test  数据库
hive > use  test ;                                   
hive > create  table score(name  string , score  int);     
#后面的这段话是指定字段的分隔符，这里设置的是  '\t'  遇到一个这个符号  即表示一个字符串结束
#COLLECTION ITEMS TERMINATED BY ','     这句话表示这个字段是一个数组，用  ',' 分割元素对象
hive > create table person(name string,age int)row format delimited fields terminated by '\t' escaped by '\\' stored as textfile;
#加载score1.txt文件中的数据到表 score 中
hive > load  data  local  inpath  ' fileshare/score1.txt '  into  table  score;
#查询显示表中的数据
hive > select  *  from score;          
#这个计算过程，Hive会自动转换成MapReduce任务执行，
hive > select  count(*)  from score;      
#删除表
hive > drop table score ;                        
```
