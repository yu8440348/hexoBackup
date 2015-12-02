title: ubuntu下Tomcat的安装
date: 2015-01-05 14:28:48
tags: Ubuntu
---


Java开发，在ubuntu系统下需要使用服务器，Tomcat服务器提供linux下版本，Tomcat小巧高效，适合于中小型企业应用。
工具/原料

    ubuntu
    apache-tomcat-7.0.5.tar.gz
<!--more-->
方法/步骤

**1. 去apache官方下载tomcat服务器软件。版本为apache-tomcat-7.0.5.tar.gz**

**2. 将下载的apache-tomcat-7.0.5.tar.gz解压放在某个文件夹内，重命名为tomcat7**
解压文件包，需要管理员权限用户。
``` bash
sudo tar zxvf  apache-tomcat-7.0.5.tar.gz
sudo mv apache-tomcat-7.05 tomcat7
```
**3. 在终端进入tomcat7下的bin文件**
``` bash
cd tomcat7/bin
sudo vim catalina.sh
```
**4. 在打开的文件中找到如下内容：**
``` bash
    cygwin=false

    os400=false

    darwin=false case "`uname`" in

    CYGWIN*) cygwin=true;;

    OS400*) os400=true;;

    Darwin*) darwin=true;;
```
**5. 在内容的上面添加如下内容：**
``` bash
    JAVA_HOME=/usr/app/java

    JAVA_OPTS="-server -Xms512m

    -Xmx1024m -XX:PermSize=600M

    -XX:MaxPermSize=600m

    -Dcom.sun.management.jmxremote"
```
**6. tomcat7.0.5的端口（一般tomcat7.0.5的端口默认为8080），如果发生冲突，则可以在以下文件修改。**
 tomcat7/conf/server.xml 文件里的：

    Connector port="8888" protocol="HTTP/1.1"  connectionTimeout="20000"  redirectPort="8443"   

改成8888，或者其它均可（这里用8888吧）。  
**7.  进入tomcat7/bin文件夹。**
``` bash
sudo ./startup.sh
```
启动Tomcat7。
**8.在浏览器中输入：http://localhost:8888**
