title: ubuntu配置vsftp
tags:
  - Ubuntu
categories: []
date: 2015-01-13 17:43:00
---
最近想在自己的vps上搭建vsftp服务器，本以为是很简单的事，但网上铺天盖地的帖子和博客，没有一个能在我的ubuntu14.04 server版的服务器上安装成功。经过半天努力，终于搞定了在64位ubuntu14.04上安装vsftpd 3.0.2，实现了禁用匿名用户和虚拟用户，只启用本地用户登录。

首先安装vsftpd:
```
sudo apt-get install vsftpd
```

然后添加ftp用户，由于只拿来登录ftp，所以为了安全把shell设置为nologin，同时把对应的家目录设置为你要让该用户访问的目录，命令如下：
添加ftp用户
```
useradd -d /var/ftp -s /usr/sbin/nologin username
```
这样就添加了一个名为username的用户，该用户不能用作系统登陆。然后修改该用户密码：
修改密码
```
passwd username
```
根据提示，连续输入两次密码后就更改了username这个用户的密码。

现在开始最关键的vsftp的配置，主要配置文件是/etc/vsftpd.conf,内容如下
/etc/vsftpd.conf
```
#禁用匿名用户登陆
anonymous_enable=NO
 
#允许本地用户登陆
local_enable=YES
 
#允许本地用户写入
write_enable=YES
 
#注意：这个地方如果不配置，就会出现只有root用户可以登陆，普通用户不可以
check_shell=NO
 
#掩码，决定了上传上来的文件的权限。设置为000使之有最大权限
local_umask=000
 
#允许记录日志
xferlog_enable=YES
 
#允许数据流从20端口传输
connect_from_port_20=YES
 
#日志路径
xferlog_file=/var/log/vsftpd.log
 
#ftp欢迎语，可以随便设置
ftpd_banner=hi,guys!
 
#注意：这个选项可以保证用户锁定在指定的家目录里，防止系统文件被修改。
chroot_local_user=YES
 
#注意：这个不配置有可能出现只能下载不能上传
allow_writeable_chroot=YES
 
#配置了可以以stand alone模式运行
listen=YES
 
#注意：该选项不配置可能导致莫名其妙的530问题
seccomp_sandbox=NO
 
#说明我们要指定一个userlist，里边放的是允许ftp登陆的本地用户。如果设置为YES，则文件里设置的是不允许登陆的本地用户
userlist_deny=NO
userlist_enable=YES
 
#记录允许本地登陆用户名的文件
userlist_file=/etc/vsftpd/allowed_users
```
最后一步就是在我们userlist_file选项指定的文件里添加允许ftp登陆的本地账户，一行写一个即可，如我的就是/etc/vsftpd/allowde_users，内容如下：
```
/etc/vsftpd/allowed_users
username
root
```
该文件说明我们允许本地用户username和root账号从ftp登陆，其他账号不可以。

好了，配置完成，下一步就是重启vsftpd服务：
重启vsftpd服务
```
service vsftpd restart
```

讲解的不详细，配置文件里标明注意的选项是关键选项，一定要配置正确才可以。祝大家成功。
> 本文转载自 http://www.while0.com/36.html