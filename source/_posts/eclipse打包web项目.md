title: eclipse打包web项目
tags:
  - Java开发
categories: []
date: 2015-01-11 13:48:00
---
- 在项目名上右键，选择export，war
- 将该文件复制到Tomcat的webapp文件夹里，启动Tomcat时，此war文件会被自动解压（如果用的是其他发布容器，同理）
- 如果想更改tomcat默认的配置，如启动端口等，可以在conf--》server.xml中修改。
- 启动是在bin中的startup.bat，如果在linux环境下，执行startup.sh吧。