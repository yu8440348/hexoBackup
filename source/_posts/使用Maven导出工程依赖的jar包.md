title: 使用Maven导出工程依赖的jar包
tags:
  - Java开发
categories:
  - 编程相关
date: 2015-01-08 16:34:00
---
- 从Maven仓库中导出jar包：进入工程pom.xml 所在的目录下，输入：

`mvn dependency:copy-dependencies  `

 > 会导出到targed/dependency 下面
 
 
- 可以在工程创建lib文件夹，输入以下命令：

`mvn dependency:copy-dependencies -DoutputDirectory=lib  `
 
>这样jar包都会copy到工程目录下的lib里面 

 
- 可以设置依赖级别，通常用编译需要的jar

`mvn dependency:copy-dependencies -DoutputDirectory=lib   -DincludeScope=compile `
