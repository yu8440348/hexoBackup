title: Ubuntu下GitHub的ssh配置
tags:
  - Ubuntu
  - git
categories: []
date: 2015-01-05 17:02:00
---
git是分布式的代码管理工具，远程的代码管理是基于ssh的，所以要使用远程的git则需要ssh的配置。
github的ssh配置如下：  
###一 、设置git的user name和email：
```bash
$ git config --global user.name "yu8440348"
$ git config --global user.email "yu8440348@163.com"
```
<!--more-->
###二、生成SSH密钥过程：
1. 查看是否已经有了ssh密钥：cd ~/.ssh
如果没有密钥则不会有此文件夹，有则备份删除
2. 生存密钥：
```
$ ssh-keygen -t rsa -C “yu8440348@163.com”
```
按3个回车，密码为空。  

Your identification has been saved in /home/yuhaitao/.ssh/id_rsa.
Your public key has been saved in /home/yuhaitao/.ssh/id_rsa.pub.
The key fingerprint is:
………………

最后得到了两个文件：id_rsa和id_rsa.pub
输出id_rsa.pub，然后复制
```
cat /home/yuhaitao/.ssh/id_rsa.pub
```

4. 在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥。
打开https://github.com/ ，登陆，然后添加ssh。

5. 测试：
```
ssh git@github.com
The authenticity of host ‘github.com (207.97.227.239)’ can’t be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added ‘github.com,207.97.227.239′ (RSA) to the list of known hosts.
ERROR: Hi tekkub! You’ve successfully authenticated, but GitHub does not provide shell access
Connection to github.com closed.
```
###三、 开始使用github
1. 获取源码：
```
$ git clone git@github.com:billyanyteen/github-services.git
```
2. 这样你的机器上就有一个repo了。
3. git于svn所不同的是git是分布式的，没有服务器概念。所有的人的机器上都有一个repo，每次提交都是给自己机器的repo
仓库初始化：
```
git init
```
生成快照并存入项目索引,-A表示提交所有文件：
```
git add -A
```
文件,还有git rm,git mv等等…
项目索引提交,必须加说明：
```
git commit -m ""
```
4. 协作编程：
将本地repo于远程的origin的repo合并，
推送本地更新到远程：
```
git push origin master
```
更新远程更新到本地：
```
git pull origin master
```
补充：
添加远端repo：
```
$ git remote add origin git://github.com/pjhyett/github-services.git
```
>origin 为分支名称

重命名远端repo：
```
$ git://github.com/pjhyett/github-services.git为“upstream”
```
![常用命令](http://7u2gdr.com1.z0.glb.clouddn.com/blogimg/203116_Uqgs_223750.png)
