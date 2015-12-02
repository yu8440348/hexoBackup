title: Pacman主题介绍
tags:
  - hexo
categories: []
date: 2015-01-06 22:32:00
---
本文转载自 http://yangjian.me/workspace/introducing-pacman-theme/ 谢谢作者

## 主题概括
Pacman是一款为Hexo打造的一款扁平化，有着响应式设计的主题。本站正是使用了该主题，同时你也可以访问Demo查看效果。主题源码托管在Github上，欢迎Fork。遇到任何问题或意见请发表issue。

## 安装指南
### 安装
```
$ git clone https://github.com/A-limon/pacman.git themes/pacman
```
Pacman需要安装Hexo 2.4.5 或以上版本 请先升级您的Hexo程序，再启用此主题。
<!--more-->
### 启用

修改你的博客根目录下的config.yml配置文件中的theme属性，将其设置为pacman。同时请设置stylus属性中的compress值为true。

### 更新
```
cd themes/pacman
git pull
```
请先备份你的_config.yml 文件后再升级

### 配置指南
Pacman主题提供了丰富的配置属性，配置文件_config.yml位于主题根目录下。配置文件中已经包含了详细的英文注释，所以下面就用中文进行说明。

属性
```
##### Menu
menu:
  Home: /
  Archives: /archives

#### Widgets
widgets:
- category
- tag
- rss

#### RSS
rss:

#### Image
imglogo:
  enable: true
  src: /img/logo.svg
  favicon: /img/favicon.ico
  apple_icon: /img/pacman.jpg

#### Author Avatar Picture
author_img_enable: true
dataURI: false
author_img_data:
author_img: /img/author.jpg

#### Font
ShowCustomFont: true  

#### Toc
toc:
  article: true
  aside: true

#### Fancybox
fancybox: true

#### Author information
author:
  google_plus:
  intro_line1:
  intro_line2:
  weibo:
  twitter:
  github:
  facebook:
  tsina:

#### Comment
duoshuo:
  enable: false        
  short_name:

#### Share button
jiathis:
  enable: false  
  id:
  tsina:

#### Analytics
google_analytics:
  enable: false
  id:
  site:

#### Custom Search
google_cse:
  enable: false
  cx:
```
### 说明

menu 默认没有启用 /tags 和 /categories 页面，如果需要启用请在博客目录下的source文件夹中分别建立tags 和 categories文件夹每个文件夹中分别包含一个index.md文件。内容为：
```
layout: tags (或categories)
title: tags (或categories)
---
```
因为主题中已经内置了这两个页面的模板，所以他们会被正确的解析出来。

widgets: 提供了6种小工具。
rss: 请填写你博客的RSS地址。
imglogo: 建议启用图片logo，格式建议为.svg或.png格式。同时建议提供配套的favicon以及在苹果设备上的图标（背景不要透明）。
author_img_enable: 是否显示底部的作者头像。主题支持头像使用dataURI格式，若使用请修改dataURI的值为true并在下面的author_img_data后填上图片的值，确保它是一行而且被引号包住如果还是想用传统的jpg格式那么就把图片路径放在author_img后，同时把dataURI设置成false。
ShowCustomFont: 启用自定义字体，如果你有一定前端基础可以修改font.styl替换为你喜欢的字体。
toc: 是否启用在文章中或侧边栏中的目录功能。二者可以都为true或都为false。同时，如果你希望在特定的某一篇文章中关闭目录功能你可以在文章文件开头中的front-matter中加上一行toc: false。
fancybox: 默认关闭，如果你使用Hexo经常发表Gallery类型的文章，那么请设置为true（同时需要复制fancybox.js到你的博客目录下scripts文件夹中）。ps: 我很佩服用Hexo发表相册的文艺青年。
author: 作者信息，建议尽量填写完整。其中tsina是你的新浪微博ID，不同于用户名或微博主页地址。启用这个属性后，其他用户在微博上分享你文章的同时会自动@你。
duoshuo: 多说评论系统。在大陆地区更好用的评论系统，如果你想更换为disqus请参考默认主题后自行修改。
jiathis: 加网分享系统。默认关闭，因为主题已经内置了原生的分享功能。
google_analytics: Google Analytics追踪代码。请注意：*Google Analytics已经升级到了Universal Analytics。请先前往后台升级你的Google Analytics版本后再启用追踪代码 更多信息请点击这里了解。
google_cse: Google自定义搜索。如果开启自定义搜索需要先登录Google CSE，配置好你的站点，并获得此自定义搜索的ID。此外你需要在博客目录下的source文件夹中建立search文件夹并包含一个index.md文件。内容为：
```
layout: search
title: search
---
```
