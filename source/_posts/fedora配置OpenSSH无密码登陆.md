title: fedora配置OpenSSH无密码登陆
tags:
  - linux
  - fedora
categories: []
date: 2015-01-27 12:58:00
---
 最近在搭建Hadoop环境需要设置无密码登陆，所谓无密码登陆其实是指通过证书认证的方式登陆，使用一种被称为"公私钥"认证的方式来进行ssh登录。  
" 公私钥"认证方式简单的解释:首先在客户端上创建一对公私钥 （公钥文件：~/.ssh/id_dsa.pub； 私钥文件：~/.ssh/id_dsa）。然后把公钥放到服务器上（~/.ssh/authorized_keys）, 自己保留好私钥.在使用ssh登录时,ssh程序会发送私钥去和服务器上的公钥做匹配.如果匹配成功就可以登录了。  
具体操作步骤如下：
<!--more-->

#### 1.确认系统已经安装好OpenSSH的server 和client
安装步骤这里不再讲述，不是本文的重点。
#### 2.确认本机sshd的配置文件(需要root权限)

    vi /etc/ssh/sshd_config
    找到以下内容，并去掉注释符”#“
    RSAAuthentication yes
    PubkeyAuthentication yes
    AuthorizedKeysFile      .ssh/authorized_keys

#### 3.如果修改了配置文件需要重启sshd服务 (需要root权限)
```
/sbin/service sshd restart    
```
#### 4.开机启动
```
systemctl enable sshd.service
```
回车会提示你输入密码，因为此时我们还没有生成证书

#### 5.生成证书公私钥的步骤：
```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
```
#### 6.修改文件authorized_keys的权限
```
chmod 644 ~/.ssh/authorized_keys
```
再次测试登陆如下：
```
ssh localhost
```
8.认证登陆远程服务器（远程服务器OpenSSH的服务当然要启动）


拷贝本地生产的key到远程服务器端（两种方法）
**方法一：**
```
$cat ~/.ssh/id_rsa.pub | ssh michael@192.168.8.148 'cat - >> ~/.ssh/authorized_keys'
```
**方法二：**
在本机上执行：
```
scp ~/.ssh/id_dsa.pub michael@192.168.8.148:/home/michael/
```
登陆远程服务器michael@192.168.8.148 后执行：
```
cat id_dsa.pub >> ~/.ssh/authorized_keys
```
本机远程登陆192.168.8.148的测试：
```
ssh michael@192.168.8.148
```
如果登陆测试不成功，需要修改远程服务器192.168.8.148上的文件authorized_keys的权限

另外，需要注意的是：
> 用户目录权限为 755 或者 700就是不能是77x
.ssh目录权限必须为755
dsa_id.pub 及authorized_keys权限必须为644
dsa_id 权限必须为600

ssh相关常用命令：
```
1>查看运行状态
/sbin/service sshd status 或 service sshd status

2>启动ssh server
/sbin/service sshd start或service sshd start

3>重启ssh server
/sbin/service sshd restart或service sshd restart
```
> 转载自 http://blog.163.com/wunan_23/blog/static/195562320201291421413681/
