title: BAE部署javaweb项目
tags:
  - Java开发
categories: []
date: 2015-01-12 14:38:00
---
用户可以上传多个WAR包或目录，java-jetty的主域名使用**root.war**，java-tomcat的主域名使用ROOT.war，其他WAR包或目录的访问需要在主域名后加上代码目录的路径，如code.war或code目录的访问：xxx.duapp.com/code/

本地开发：

1.	使用eclipse开发
下载eclipse： http://www.eclipse.org/downloads/
打开eclipse，新建Dynamic Web Project
开发完成后，打成WAR包，File->Export->WAR file, 保存为root.war（java-jetty）或ROOT.war（java-tomcat），通过SVN或GIT上传到BAE
或者将root.war或ROOT.war解压到root或**ROOT**目录下，然后删除原svn中的root.war或ROOT.war，再将root或**ROOT目录**通过SVN或者GIT上传到BAE.

> BAE 上传路径中只有根目录能放jsp文件，或者新建一个jsp文件夹来放文件。静态文件必须先建立static文件夹，然后再将静态文件拷贝到static目录，否则再bae中无法访问资源。
> 也可以建立一个bae云存储，将静态文件放到云存储中，然后设为公开文件，然后再项目中引用云存储的文件。

