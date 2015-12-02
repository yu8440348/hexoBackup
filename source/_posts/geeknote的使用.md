title: geeknote的使用
tags:
  - linux
categories: []
date: 2015-01-27 11:12:00
---
### GeekNote

#### 正文

但是这个世界上就是有很多的和我这样的有这种需求，但比我牛很多的人存在。所以我就受益了(想想不免自卑了)。我发现的这个是在linux中的，叫做geeknote,看名字就知道了。geek范儿的，脱离了界面的支持，这对我这样习惯了linux下命令行的人还是比较喜欢的。好了废话不多说。开始介绍了。
安装

geeknote的官网，我们可以看到一些介绍，安装的步骤还是随大流的安装，但是一点需要注意的就是。Eernote是国外的而印象笔记是国内的，虽然是一家人，但是服务器不一样，账户也是又差别的。因此需要对自己的所使用的账户要有所区分。
```
    印象笔记用户：git clone git://github.com/gmajian/geeknote.git
    Evernote用户：git clone git://github.com/VitaliyRodnenko/geeknote.git
```
这是第一步,这一步很关键的，如果你是印象笔记用户，却下载的Evernote版本的话，那么就用不了。BTW，你的网还要好一点，不然在安装过程中，会有些下载不下来。好比我用的校网死活下不下来，但是chinanet给了我希望^_^.
<!--more-->
其他的安装部分如下：

```
sudo python setup.py install
```
安装好了后，我们需要将geeknote和印象笔记相互连接，授权一下，我们使用geeknote login然后输入用户名和密码，但是还会出现一个Code当时我没有反应过来这是啥？直接回车了。后来发现这个没啥事儿。我搜了很久也没发觉这个是啥。如果你恰好知道的话，希望你能告诉我，先谢谢拉～

当我们授权完成后会有提示successfully那么我们可以查看下是否是真的授权成功了geeknote settings 会出现如下内容：

```
Geeknote
******************************
Version: 0.1
App dir: /Users/username/.geeknote
Error log: /Users/username/.geeknote/error.log
Current editor: vim
******************************
Username: username
Id: 11111111
Email: example@gmail.com
```
你主要是要看你的username是否正确，因为登录的时候使用的邮箱登录的，所以用户名对了的话，那基本上没有多大问题了。

还有一点要说明的就是设置编辑器，我是一个vim党，所以我习惯使用的是vim 所以我用geeknote settings --editor vim来设置默认的编辑器。但是有人推荐使用是retext，这是linux下编辑markdown的软件。实时预览功能，安装sudo apt-get install retext 然后和上面一样的操作。
使用
笔记

1.创建笔记
```
geeknote create --title "xxx" --notebook "xxxx" --content "WRITE"  --tags "xx,xx,xx"
```
需要注意的是如果你不想大篇幅的去编辑一个文件的话，那么在content后面直接加上你要填入的内容。上述的参数可以根据个人自己随意选择的。

2.修改笔记
```
geeknote edit --note "xxx" --content "WRITE"
```
上面这是最简单的修改了，就是修改下内容而已。如果你想去修改笔记的标题，所处笔记本，以及标签的话使用如下：
```
geeknote edit --note "xxx" --title "xxxx"   重命名文件
geeknote edit --note "xxx" --notebook "xxxx"  更改笔记本
geeknote edit --note "xxx" --tags "xxx,xxx"  更改标签
```
3.搜索笔记

使用geeknote时，如果是使用的create来创建文件的话，那么只是一个临时的文件，写完后直接同步过去了，所以我们一般是在本地找不到我们的笔记的。能做的就是在database中搜索出我们的笔记存档，然后连接服务器获取。因此这个搜索操作还是蛮重要的。
```
geeknote find --search <text to find>
                [--tags <list of tags that notes should have>]
                [--notebooks <list on notebooks where to make search >]
                [--date <data ro data range>]
                [--count <how many results to show>]
                [--exact-entry]
                [--content-search]
                [--url-only]
```
使用还是比较简单的，根据需求选择参数。

4.在终端中显示笔记

我们有时只是想看看笔记的内容是什么。所以可以类似于cat的一种操作，那就是使用show。用法如下:
```
geeknote show <text to search and show>
```
简单说两个例子：
```
geeknote show "geeknote"   如果笔记同名的比较多会有选项让你选择
geeknote find --search "geeknote"
geeknote show 2    这是两步操作，显示上面查找的第二个文件
```
一般来说，如果文件内容比较多的话，使用less来查看还是不错的选择。

5.删除笔记

删除笔记还是比较简单的，简单干脆
```
geeknote remove --note "xxxx"
```
笔记本

1.查看笔记本中的笔记
```
geeknote notebook-list  
```
2.创建笔记本
```
geeknote notebook-create --title "xxx"
```
3.修改笔记本名
```
geeknote notebook-edit --notebook <old name> --title "xxx"
```
标签

1.展现所有的标签
```
geeknote tag-list
```
2.创建标签
```
geeknote tag-create --title "xxxx"
```
3.重命名
```
geeknote tag-edit --tagname "xxx" --title "xxx"
```
4.移除标签
```
geeknote tag-remove --tagname "xxx"
```  
#### gnsync

使用gnsync是将本地的md的笔记同步到印象笔记中，所同步的内容会放在统一割笔记本中的。所以使用这个gnsync可以做一个同步的工作，效果还是不错的。

简单的语法是这样的：
```
gnsync --path ~/path/to/sync/ --mask "*.md" --format markdown
```
上面这个基本上就足够了。直接同步到印象笔记上去了。但是只能同步text data，所以限制还是不少的。想要geek就得啥都geek起来：）

> 转载自 http://jesseeisen.github.io/2014/10/11/markdown-evernote.html
