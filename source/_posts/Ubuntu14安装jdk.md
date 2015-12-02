title: Ubuntu14安装jdk
date: 2015-01-05 13:50:50
tags:
- Ubuntu
categories: 编程相关
---
**1. 到 Sun 的官网下载**  
``` bash
http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
```
**2. 解压文件，修改文件名**    
``` bash
$ sudo mkdir /usr/app
$ sudo tar zxvf jdk-7u21-linux-i586.tar.gz -C /usr/app
$ cd /usr/app
$ sudo mv jdk1.7.0_21 java
```
<!--more-->
**3. 添加环境变量**  
``` bash
$ sudo vim /etc/profile
```
**加入如下内容**
``` bash
export JAVA_HOME=/usr/lib/jvm/java
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH  
```

**4. 配置默认JDK版本**  
``` bash
sudo update-alternatives --install /usr/bin/java java /usr/app/java/bin/java 300  
sudo update-alternatives --install /usr/bin/javac javac /usr/app/java/bin/javac 300  
sudo update-alternatives --install /usr/bin/jar jar /usr/app/java/bin/jar 300   
sudo update-alternatives --install /usr/bin/javah javah /usr/app/java/bin/javah 300   
sudo update-alternatives --install /usr/bin/javap javap /usr/app/java/bin/javap 300
```

**然后执行**
``` bash
sudo update-alternatives --config java
```

**若是初次安装 JDK， 将提示**
``` bash
There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/java/bin/java
无需配置。
```
**4. 测试**  
``` bash
$ java -version
java version "1.7.0_21"
Java(TM) SE Runtime Environment (build 1.7.0_21-b11)
Java HotSpot(TM) Server VM (build 23.21-b01, mixed mode)
```
