title: Fedora安装以后的优化
tags:
- linux
- fedora
categories: []
date: 2015-01-27 11:38:00
---
#### 1.安装gnome-tweak-tool设置工具
Fedora 19自带的系统设置工具十分简单，一些重要的地方都不能设置。比如窗口默认没有最大化和最化小的按钮。

```bash
sudo yum install gnome-tweak-tool
```

然后在左上角的“活动”里找到并打开“优化工具”，在左侧选择“窗口”，在右侧找到“Titlebar Buttons”，把下面的“Maximize”和“Minimize”打开，这样，窗口的右上角就有最大化和最小化按钮了。

此外，还可以在左侧的“Shell”中，把“在日历中显示星期”打开。

对于国内用户，肯定不习惯多个桌面的形式，打开“优化工具”，在左侧选择“Workspaces”，把右侧的“Workspaces Creation”设置为“Static”，下面的“Number of Workspaces”设置为1，这样就只有1个桌面了。

<!--more-->

#### 2.设置网易软件源设置最快软件源
把yum-plugin-fastestmirror插件装上就行了，没有必要安装网易的源。

```bash
sudo yum install yum-plugin-fastestmirror
```

#### 3.常用软件安装
安装FTP客户端(Filezilla)：

```bash
sudo yum install filezilla
```

安装flash插件(32位)：

```bash
sudo rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
sudo yum install flash-plugin
```

安装flash插件(64位)：

```bash
sudo rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm
sudo yum install flash-plugin

sudo yum install nautilus-open-terminal  #为右键菜单添加“在终端中打开”选项

sudo yum install libreoffice-langpack-zh-Hans #给LibreOffice安装中文字体
```

Fedora 19自带了LibreOffice，可以打开一般Word、Excel、PPT文档。此外，系统还自带了一个“文档查看器”，可以打开PDF格式的文件。

```bash
sudo yum install gnome-shell-extension-user-theme
sudo yum list | grep gnome-shell-theme      #查看系统可用的主题
sudo yum install gnome-shell-theme-zukitwo  #为系统安装zukitwo透明主题
```

安装完主题以后，打开“优化工具”，选择左侧“Extensions”，将“User themes”开启。然后选择左侧“Appearance”，将右侧的“Shell 主题”切换成“zukitwo”，然后就会发现，系统的状态栏变成透明了。如果这里的“Shell 主题”显示的是一个感叹号，重启一下就可以设置了。

#### 4.安装五笔输入法（ibus）

```bash
[bear@bogon ~]$ sudo yum list | grep wubi
ibus-table-chinese-wubi-haifeng.noarch  1.4.6-2.fc19                     fedora
ibus-table-chinese-wubi-jidian.noarch   1.4.6-2.fc19                     fedora
[bear@bogon ~]$ sudo yum install ibus-table-chinese-wubi-haifeng
```

然后需要重启一下（不知道为什么需要重启），添加一个输入源。打开“设置”－“区域和语言”，点击“输入源”下面的加号，选择“汉语（中国）”，列表里会出现“汉语（海峰五笔86）”，然后点“添加”就可以了。
简单测试了一下，貌似海峰五笔比极点五笔好用一些。极点五笔把tfrc打出了“造反”，而海峰五笔可以同时打出为“造反”和“选择”。
（补充：发现极点五笔确实很差，ykkl无法打出“识别”）

#### 5.安装多媒体播放器（vlc）

```bash
sudo rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo yum install vlc
```

vlc这个万能播放器怎么能少呢？装完就可以播放常见的多媒体文件了。还可以把系统默认的播放器设置为vlc，这个就不用多说了。

#### 6.安装音乐播放器
百度了一下，比较好的音乐播放器有
Clenmentine，功能较为强大，有点儿像iTunes
audacious，界面简洁，比较像foobar
这里以安装audacious为例：

```bash
yum install audacious
yum install audacious-plugins-freeworld  #安装mp3解码器，需要RPM Fusion，在本文第5步中已经安装
```

#### 7.设置桌面快捷方式
将本文中第一步安装的“优化工具”打开，在左侧选择“桌面”，将右侧的“Icons on Desktop”打开，这样的话，桌面上就会出来图标了。

如何将喜欢的应用程序建立桌面图标？
打开/usr/share/applications/，找到你喜欢的应用程序图标，右键选择“复制到…”，弹出的窗口中，左侧选择“Home”，再在右侧选“桌面”即可。

#### 8.安装QQ2012
请参考教程：http://tieba.baidu.com/p/1839029331

#### 9.字体设置
Fedora 19默认的点阵字体感觉非常不好，尤其是在大屏幕的显示器上。所以我们需要换一种字体。
首先说一下字体的安装方法，这里以安装文泉驿微米黑为例：

```bash
sudo mkdir /usr/share/fonts/wqy-microhei  #建立字体目录
wget http://sourceforge.net/projects/wqy/files/wqy-microhei/0.2.0-beta/wqy-microhei-0.2.0-beta.tar.gz
tar -zxvf wqy-microhei-0.2.0-beta.tar.gz  #将下载的字体解压
cd wqy-microhei/
sudo mv wqy-microhei.ttc /usr/share/fonts/wqy-microhei
fc-cache -fv    #更新字体缓存
```

然后在“字体查看器”中已经可以找到这个字体了。
然后打开“优化工具”，选择左侧的“字体”，将“默认字体”改为“文泉驿微米黑”，然后，就会发现整个系统的字体变了。
我把Windows系统中常见的字体整理了一份，包括宋体，微软雅黑，黑体，仿宋，楷体，黑体等。下载地址请点击此处，安装方式不再多说。

关于字体，2014.01.05补充：
发现了两个非常好看的字体：文泉驿正黑体和google-droid-sans-fonts，尤其对中文的支持近乎完美。我的Fedora 20 64bit已经自带了这两种字体。如果你的系统里没有，也可以使用如下方法安装：

```bash
sudo yum install google-droid-sans-fonts
sudp yum install wqy-zenhei-fonts
```

然后打开“优化工具”，将“字体”改为文泉驿正黑，然后，享受美观的中文字体吧。

补充：发现由红帽支持的Liberation Fonts字体比较美观，十分接近windows XP的样子。Fedora19中已经自带了这种字体。这里给出字体方案：
默认字体：Liberation Sans 10
文档字体：Liberation Sans 10
等宽字体：Liberation Mono 10

2014.04.25补充：
很多字体在fedora上显示的都很模糊，使用开源字体渲染软件Infinality来解决吧：

```bash
sudo rpm -Uvh http://www.infinality.net/fedora/linux/infinality-repo-1.0-1.noarch.rpm
sudo yum install freetype-infinality fontconfig-infinality
vim /etc/profile.d/infinality-settings.sh   #设置渲染风格，默认的default就很清晰了
USE_STYLE="DEFAULT"
```

然后重启即可。

关于Infinality的用法：

```bash
sudo vim /etc/profile.d/infinality-settings.sh  #设置渲染风格，有win7/winXP/linux/osx等
sudo vim sudo vim /etc/fonts/infinality/styles.conf.avail/win7/20-aliases-default-win.conf #修改win7风格下的字体
```

在Infinality配置文件中设置字体的时候，可能需要获得字体的名称，如果中文字体要怎么设置名称呢？首先安装fontconfig软件包，然后在程序中找到“字体查看器”，这时候就可以找到中文字体的英文名称了。

#### 10.强大的Gnome Shell插件
借助强大的Gnome Shell插件，可以实现很多特殊的功能，比如在顶部的状态栏显示所有已打开的应用程序。个人认为这个功能对于提升效率非常有帮助。简单查找了一下，找到以下3个插件可以实现这个功能：
Window List：这个貌似是最好的，但目前不支持最新的Gnome3.8。
TaskBar：很多小功能非常帖心，比如带有“显示桌面”的小图标，以及显示所有程序列表的小图标。推荐！
Yet Another Window List：功能一般，但据说速度比前者好，只是听说，本人在实际使用过程中觉得速度上没有差别，但前者的使用体验要优于它。

Gnome Shell插件使用起来非常简单，在Fedora系统自带的Firefox中打开插件地址，页面左上角会出现一个OFF的图标，把点改成ON即可。

#### 11.更换主题
```bash
sudo yum install faenza-icon-theme  #安装faenza图标主题
```
然后打开“优化工具”，选择左侧的“主题”，将“图标主题”改为“Faenza”即可。

#### 21.安装解压软件

```bash
sudo rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
sudo rpm -Uvh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
sudo yum install unrar
```

fedora 中怎么解压rar文件呢？很简单，就是 unrar e file_name.rar

>转载自 http://www.zhukun.net/archives/6614


