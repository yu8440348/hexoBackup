title: hbase伪分布式安装配置
tags:
  - hbase
categories: []
date: 2015-01-23 17:43:00
---
## hbase伪分布式安装配置
> java版本：jdk1.7  
hadoop版本：2.6.0  
hbase版本：hbase-0.98.9-hadoop2

### hbase-env.sh里的配置
```
# java安装路径
export JAVA_HOME=/usr/app/java/

# hadoop配置文件路径
export HBASE_CLASSPATH=/home/yuhaitao/hadoop/etc/hadoop/

# The maximum amount of heap to use, in MB. Default is 1000.
export HBASE_HEAPSIZE=1000
```
<!--more-->
### 配置hbase-site.xml
```xml
<configuration>
 <property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
    <description>数据库文件路径
    </description>
  </property>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
    <description>此参数指定Hlog和Hfile的副本个数，由于时伪分布式，所以只能时1个节点，不能大于1个节点
    </description>
  </property>
<property>   
<!-- hbase 的 tmp 目录 -->  
<name>hbase.tmp.dir</name>   
<value>/home/yuhaitao/hadoop/hbase-tmp</value>
</property>
<property>     
	<name>hbase.master</name>   
	<value>localhost:60000</value>
</property>
</configuration>
```
### 启动hbase
- 先启动 hadoop ，然后启动habse
```
~/hadoop/sbin$ ./start-all.sh
~/hadoop/hbase-hadoop2/bin$ ./start-hbase.sh
jps
13437 DataNode
13662 SecondaryNameNode
14488 Jps
13953 NodeManager
13821 ResourceManager
14396 HMaster
13301 NameNode
#打开HBase命令行客户端访问Hbase
bin/hbase shell
```
> 启动成功后可以打开web管理页面 http://localhost:60010/

### habse简单操作
```bash
#创建一个新表student
hbase(main):003:0> create 'student','info'
0 row(s) in 1.2680 seconds

#查看所有的表
hbase(main):004:0> list
TABLE
student
1 row(s) in 0.0330 seconds

#查看student的表结构
hbase(main):005:0> describe 'student'
DESCRIPTION                                                 ENABLED
 'student', {NAME => 'info', DATA_BLOCK_ENCODING => 'NONE', true
  BLOOMFILTER => 'NONE', REPLICATION_SCOPE => '0', VERSIONS
  => '3', COMPRESSION => 'NONE', MIN_VERSIONS => '0', TTL =
 > '2147483647', KEEP_DELETED_CELLS => 'false', BLOCKSIZE =
 > '65536', IN_MEMORY => 'false', ENCODE_ON_DISK => 'true',
  BLOCKCACHE => 'true'}
1 row(s) in 0.1100 seconds

#同student表中插入一条数据
hbase(main):007:0> put 'student','mary','info:age','19'
0 row(s) in 0.0490 seconds

#从student表中取出mary的数据
hbase(main):008:0> get 'student','mary'
COLUMN                   CELL
 info:age                timestamp=1396366643298, value=19
1 row(s) in 0.0190 seconds

#让student表失效
hbase(main):009:0> disable 'student'
0 row(s) in 1.2400 seconds

#列出所有表
hbase(main):010:0> list
TABLE
student
1 row(s) in 0.0310 seconds

#删除student表
hbase(main):013:0>  drop 'student'
0 row(s) in 1.1100 seconds

#列出所有表
hbase(main):014:0> list
TABLE
0 row(s) in 0.0400 seconds
```
