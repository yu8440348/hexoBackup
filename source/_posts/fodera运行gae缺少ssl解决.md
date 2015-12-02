title: fodera运行gae缺少ssl解决
tags: linux
date: 2015-01-27 16:34:11
---
如果运行gae出现以下错误
```
[yuhaitao@localhost local]$ sudo python proxy.py
[sudo] password for yuhaitao: 
Traceback (most recent call last):
  File "proxy.py", line 93, in <module>
    import OpenSSL
ImportError: No module named OpenSSL

```
执行以下命令安装ssl
```
sudo yum install pyOpenSSL
``` 

若是打开chrome提示“您的链接不是私秘的”

打开浏览器-设置-证书管理-授权中心 导入local下的证书文件ca.crt,点击所有信任

或者修改local下proxy.ini 
```
[gae]

enable = 1

appid = yu8440348

password =

path = /_gh/
#这里改成http
mode = https

ipv6 = 0

sslversion = TLSv1

window = 7

cachesock = 1

headfirst = 1

keepalive = 0

obfuscate = 0

validate = 0

transport = 0

options =

regions =
```