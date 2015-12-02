title: Fedora安装快盘
tags:
  - fedora
categories: []
date: 2015-01-29 15:51:00
---
- 进入官网 下载ubuntu版本的客户端（注意系统位数）  
解压deb包，可看到DEBIAN,etc,usr,opt这几个文件夹（如只看到data.tar压缩包，则解压这个压缩包，可看到这几个文件夹）
进入解压后文件夹根目录，将usr,opt文件夹复制到系统根目录下
```
    # cp -r usr /
    # cp -r opt /
```
<!--more-->
- 安装库文件
```
    # yum install libqxt
    # yum install boost-iostreams
```
- 需要什么库文件可以通过在终端启动快盘时显示的错误信息排查。如果不是这两个的话也是正常情况。

- 创建链接
```
    # ln /usr/bin/kuaipan4uk /home/yourhomedirectory/anywhereyoulike
```
如果提示缺少boost-iostreams1.54.0,执行以下命令，指向高版本的库
```
sudo ls usr/lib64/libboost_iostreams.so.1.55.0 usr/lib64/libboost_iostreams.so.1.54.0
```
- 此时双击该链接即可启动快盘

> 本文转载自 http://www.jianshu.com/p/825cc74f0a73
