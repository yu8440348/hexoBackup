title: linux解压缩war包
tags:
  - Ubuntu
  - Java开发
categories: []
date: 2015-01-11 00:34:00
---
解压缩war包：
把当前目录下的所有文件打包成game.war
``` 
jar -cvfM0 game.war ./
```
- -c   创建war包
- -v   显示过程信息
- -f    
- -M
- -0   这个是阿拉伯数字，只打包不压缩的意思
 
解压game.war
```
jar -xvf game.war
```
解压到当前目录