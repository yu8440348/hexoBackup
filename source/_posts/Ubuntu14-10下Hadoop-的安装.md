title: Ubuntu14.10下Hadoop 单机的安装
tags:
  - Ubuntu
  - Hadoop
categories: []
date: 2015-01-14 16:45:00
---
## 环境
- 系统： Ubuntu 14.04 64bit
- Hadoop版本： Hadoop 2.6.0 (stable)
- JDK版本： sun jdk7

## 安装SSH server、配置SSH无密码登陆
### 安装 ssh 和 rsync
> ssh 是一个很著名的安全外壳协议 Secure Shell Protocol。 rsync 是文件同步命令行工具
<!--more-->
```
sudo apt-get install ssh rsync
```  
### 配置 ssh 免登录
```
ssh-keygen -t dsa -f ~/.ssh/id_dsa
```
> 执行这条命令生成 ssh 的公钥/私钥,执行过程中,会一些提示让输入字符,直接一路回车就可以

```
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```
> ssh 进行远程登录的时候需要输入密码,如果用公钥/私钥方式,就不需要输入密码了。上述方式就是设置公钥/私钥登录。

```
ssh localhost
```
> 第一次执行本命令,会出现一个提示,输入 ” yes”然后回车即可

## 安装Hadoop 2.6.0
下载Hadoop 2.6.0后,解压到/usr/local/中。
```
sudo tar -zxvf ~/下载/hadoop-2.6.0.tar.gz -C /usr/local   # 解压到/usr/local中
sudo mv /usr/local/hadoop-2.6.0/ /usr/local/hadoop      # 将文件名改为hadoop
sudo chown -R yuhaitao:yuhaitao /usr/local/hadoop       # 修改文件权限
```
> yuhaitao 为当前登录用户

Hadoop解压后即可使用。输入如下命令Hadoop检查是否可用，成功则会显示命令行的用法：
```
/usr/local/hadoop/bin/hadoop
```
### Hadoop单机配置
```
cd /usr/local/hadoop
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep input output 'dfs[a-z.]+'
cat ./output/*
```
执行成功后输出的结果是符合正则的单词dfsadmin出现了1次  
再次运行会提示出错，需要将./output删除。
```
rm -R ./output
```

### 配置java路径
编辑 hadoop/etc/hadoop/hadoop-env.sh, 修改java路径
```
# The java implementation to use.
export JAVA_HOME=/usr/app/java
```
### 配置环境变量
> 为什么是profile文件，而不是~/.bashrc文件
/etc/profile:此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.并从/etc/profile.d目录的配置文件中搜集shell的设置.
~/.bashrc:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该文件被读取.（每个用户都有一个.bashrc文件，在用户目录下）
另外,/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承/etc/profile中的变量,他们是”父子”关系.
Hadoop可以在单节点上以伪分布式的方式运行，Hadoop进程以分离的Java进程来运行，节点即是NameNode也是DataNode。需要修改2个配置文件etc/hadoop/core-site.xml和etc/hadoop/hdfs-site.xml。Hadoop的配置文件是xml格式，声明property的name和value。

vim /profile 设置环境变量
```
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
```
3:让文件生效
```
source /etc/profile
```
输入 hadoop，显示如下，表示配置成功
```
Usage: hadoop [--config confdir] COMMAND
       where COMMAND is one of:
  fs                   run a generic filesystem user client
  version              print the version
  jar <jar>            run a jar file
  checknative [-a|-h]  check native hadoop and compression libraries availability
  distcp <srcurl> <desturl> copy file or directories recursively
  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive
  classpath            prints the class path needed to get the
  credential           interact with credential providers
                       Hadoop jar and the required libraries
  daemonlog            get/set the log level for each daemon
  trace                view and modify Hadoop tracing settings
 or
  CLASSNAME            run the class named CLASSNAME

Most commands print help when invoked w/o parameters.

```

修改配置文件etc/hadoop/core-site.xml，将
```
<configuration>
</configuration>
```
修改为下面配置：
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
修改配置文件etc/hadoop/hdfs-site.xml为
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/data</value>
    </property>
    #启用webhdfs，可用web接口管理hdfs文件系统
    <property>
           <name>dfs.webhdfs.enabled</name>
           <value>true</value>
        </property>
</configuration>
```
关于配置的一点说明：上面只要配置 fs.defaultFS 和 dfs.replication 就可以运行，不过有个说法是如没有配置 hadoop.tmp.dir 参数，此时 Hadoop 默认的使用的临时目录为 /tmp/hadoo-hadoop，而这个目录在每次重启后都会被干掉，必须重新执行 format 才行（未验证），所以伪分布式配置中最好还是设置一下。此外也需要显式指定 dfs.namenode.name.dir 和 dfs.datanode.data.dir，否则下一步可能会出错。

配置完成后，首先初始化文件系统 HDFS:
```
bin/hdfs namenode -format
```

> 成功的话，最后的提示如下，Exitting with status 0 表示成功，Exitting with status 1: 则是出错。若出错，可试着加上 sudo, 既 sudo bin/hdfs namenode -format 试试看。

接着开启NaneNode和DataNode守护进程。
```
sbin/start-dfs.sh
```
若出现下面SSH的提示，输入yes即可。

> 成功启动后，可以通过命令jps看到启动了如下进程NameNode、DataNode和SecondaryNameNode。

> 此时可以访问Web界面http://localhost:50070来查看Hadoop的信息。

## Hadoop伪分布式实例-WordCount
首先创建所需的几个目录
```
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/hadoop
```
接着将etc/hadoop中的文件作为输入文件复制到分布式文件系统中，即将/usr/local/hadoop/etc/hadoop复制到分布式文件系统中的/user/hadoop/input中。上一步创建的 /user/hadoop 相当于 HDFS 中的用户当前目录，可以看到复制文件时无需指定绝对目录，下面的命令的目标路径就是 /user/hadoop/input:
```
bin/hdfs dfs -put etc/hadoop input
```
运行MapReduce作业，执行成功的话跟单机模式相同，输出作业信息。
```
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep input output 'dfs[a-z.]+'
```
查看运行结果
```
bin/hdfs dfs -cat output/*
```
也可以将运行结果取回到本地。
```
rm -R ./output
bin/hdfs dfs -get output output
cat ./output/*
```
结束Hadoop进程，则运行
```
sbin/stop-dfs.sh
```
> 注意  
下次再启动hadoop，无需进行HDFS的初始化，只需要运行 sbin/stop-dfs.sh 就可以！

## 附加教程
解决 sbin/start-dfs.sh中的warn提示

提示ssh: Could not resolve hostname *: Name or service not known

首先输入命令hostname看下自己的机器名，如我这边是powerxing-M1。修改/etc/hosts，将127.0.1.1 powerxing-M1改成192.168.1.121 powerxing-M1，即本机地址。

提示WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform… using builtin-java classes where applicable

这是因为hadoop native library是32位系统编译的，在64位系统上会有这个提示，需要下载hadoop的源码重新编译，可参考  http://stackoverflow.com/questions/19943766/hadoop-unable-to-load-native-hadoop-library-for-your-platform-error-on-centos

我已经在Ubuntu 14.04 64bit上编译过了，可下载我编译的进行覆盖。

下载地址: http://pan.baidu.com/s/1c0AJ3Gk

下载后执行如下命令，替换掉原来的navtive library。
```
rm -R /usr/local/hadoop/lib
tar -zxvf ~/下载/lib.tar.gz -C /usr/local/hadoop
```
## 编译 Hadoop 源码
若想自己编译源码，需安装 g++ 和 Protocol Buffers（若访问不了可点此下载http://pan.baidu.com/s/1ntIAoVR）：
```
sudo tar -zxvf ~/下载/protobuf-2.5.0.tar.gz -C /usr/local
sudo apt-get install g++
cd /usr/local/protobuf-2.5.0
sudo ./configure
sudo make
sudo make check
sudo make install
protoc –-version
```
最后一行代码应输出 protoc 的版本信息，若不能运行，则执行：
```
sudo ldconfig
protoc –-version
```
若提示出错错误 “protoc: error while loading shared libraries: libprotoc.so.8: cannot open shared object file: No such file or directory”。则执行：
```
export LD_LIBRARY_PATH=/usr/local/lib/
```
安装后切换到 Hadoop 源码目录下，执行：
```
cd ~/hadoop-2.4.1-src
mvn package -Pdist,native -DskipTests -Dtar
```
编译完成后，在 hadoop-2.4.1-src/hadoop-dist/target 可看到。

## hadoop 集群配置
http://www.powerxing.com/install-hadoop-cluster-2-4-1/

> 本文转载自 http://www.powerxing.com/install-hadoop-2-4-1-single-node/
