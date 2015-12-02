title: Ubuntu上安装hexo
tags:
  - Ubuntu
  - hexo
date: 2015-01-06 21:04:29
---
## ubuntu上安装本地环境

本文转载自 http://blog.maxwi.com/2014/02/22/first-post/ 版权归作者所有

hexo 依赖于Node.js和Git所以下面分别开始安装这两个软件

## 安装Git

ubuntu安装git直接apt-get就可以了
```
$ sudo apt-get install git-core
```
<!--more-->
## 安装Node.js

hexo官方推荐的安装方法是使用nvm，这里我们也使用nvm进行安装，当然你也可以使用直接安装，不过貌似nvm安装之后会直接在个人目录下产生.nvm目录并且通过.bashrc或者.bash_profile进行开机加载，然后其他的nvm操作都会保存在>.nvm目录，这样方便以后升级或者重装系统，相当于绿色软件了。
nvm的github主页https://github.com/creationix/nvm  
安装nvm可以使用以下两个命令中的任意一个都可以
```
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```
或者Wget:
```
$  wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```
等待nvm安装完成之后重新启动你的终端然后运行以下命令安装Node.js
```
$ nvm install 0.10
```
我这里安装的node.js版本是0.10.26可用使用nvm ls命令查看
至此本地环境安装完成
安装并初始化Hexo

### 使用npm命令安装hexo

```
$ npm install -g hexo
```
这里有两点需要提醒一下：

- 1、如果提示command not found，请检查是否已经重新启动终端或者使用nvm ls检查当前使用的node.js的版本，如果没有则使用以下命令来使用刚安装的版本。

```
$ nvm use 0.10.26
```

或者使用以下命令直接设置全局的默认node.js版本
```
$ nvm alias default 0.10.26
```
### 使用hexo命令对hexo进行初始化
（这里我位于~目录，而且我想把我的个人博客放在~/hexo目录，需要放在其他目录直接改一下自己需要的目录就可以了）
```
mkdir hexo
cd hexo
hexo init hexo
```
初始化目录

```
npm install
#安装git插件
npm install hexo-deployer-git --save
```
现在本地版本的hexo已经配置完成了，可以使用以下命令来生成静态文件
```
hexo generate
```
或者
```
hexo g
```

使用以下命令启动本地服务器进行预览
```
hexo s
```
然后通过
> http://localhost:4000/  
进行访问，如果页面正常打开，那么恭喜你，你的本地博客已经搭建完成，还差一点点就可以进行发布了。

### 配置git并发布

配置git并发布基于hexo和github的个人博客
首先编辑你hexo安装目录下的_config.yml文件，找到以下内容并修改为github
```bash
    deploy:
    type: github
    repository: https://github.com/username/username.github.io.git
    branch: master
```
运行以下命令设置你的git全局变量,即设置你的用户名和邮箱
```
git config --global user.name "Your Name Here"  
git config --global user.email "your_email@example.com"
```
好了，现在已经可以使用以下命令将你的博客发布到github上了，当然需要根据提示输入你的用户名和密码
```
hexo deploy
```
或者
```
hexo d
```
记得每次运行hexo deploy之前先运行hexo generate生成你本地博客的最新版本
如果需要将博客页面中的个人信息修改成你自己的信息，请编辑并认真查看~/hexo目录下的_config.yml文件，强烈推荐多查看官方帮助文档中的说明。关于主题的修改及配置，还有添加其他RSS插件、多说插件等，请参见其他文章。
### 安装插件
#### sitemap & rss

切换到博客根目录下，输入：
```bash
cd hexo
npm install hexo-generator-feed

npm install hexo-generator-sitemap
```
之后重启博客，访问/atom.xml和/sitemap.xml，会发现已经生成了。可以把sitemap提交到搜索引擎的站长平台来增加收录
#### 备份插件
```
npm install hexo-git-backup --save
```
#### 文章编辑插件
```
npm install --save hexo-admin-plugin
```
