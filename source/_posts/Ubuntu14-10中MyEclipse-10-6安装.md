title: Ubuntu14.10中MyEclipse 10.6安装
tags:
  - Ubuntu
categories: []
date: 2015-01-07 13:38:00
---
## 安装MyEclipse
示例路径：下载/myeclipse.run
 
示例文件名：myeclipse.run
- ctrl+alt+t打开终端，切换到myeclipse所在路径
- 设置myeclipse.run的执行权限，使之可以安装： 
```bash
	sudo chmod +x myeclipse.run   
```
- 运行myeclipse安装向导（执行安装向导之前不要忘了先JDK，JDK1.7安装教程http://www.linuxidc.com/Linux/2012-11/74189.htm）：  
```bash
sudo sh myeclipse.run
```
- OK，进入图像界面安装myeclipse,(选择安装路径，选择系统，我这里64位的),

## 建立快捷方式,正常启动程序
- 在执行myeclipse之前，我们要给它赋上root权限：
```bash
cd /usr/app
sudo chown -R root:root MyEclipse
sudo chmod -R +r MyEclipse
cd 'MyEclipse/MyEclipse 10/'
sudo chown -R root:root myeclipse
sudo chmod -R +r myeclipse
```
- 建立myeclipse的执行文件，类似于windows中的.exe文件
```bash
sudo vim /usr/bin/MyEclipse
```
- 设置文件权限MyEclipse文件权限
```bash
sudo chmod 755 -R /usr/bin/MyEclipse
```
- 在Dash 面板添加MyEclipse 10快捷方式 
```
sudo gedit /usr/share/applications/MyEclipse.desktop 
```
执行命令后自动打开编辑器，写入如下内容目录改成自己的，引号为英文状态下的，Icon可以任意更换
```bash
[Desktop Entry]
Encoding=UTF-8
Name=MyEclipse 10
Comment=IDE for JavaEE
Exec=/usr/MyEclipse/MyEclipse\ 10/myeclipse
Icon=/usr/MyEclipse/MyEclipse 10/icon.xpm
Terminal=false
Type=Application
Categories=GNOME;Application;Development;
StartupNotify=true
```
- 为所有用户赋予执行权限
```bash
sudo chmod -R 777 /usr/app/MyEclipse
```

- 最后一步，初始化启动一下：
```bash
'/usr/MyEclipse/MyEclipse 10/myeclipse' -clean
```
- 代码提示闪退问题解决，修改/usr/app/MyEclipse/myeclipse.ini文件
```
-Xmx512m
-XX:MaxPermSize=512m
-XX:ReservedCodeCacheSize=256m
-Dosgi.nls.warnings=ignore
```
然后在此文件中追加：
```
-Dorg.eclipse.swt.browser.DefaultType=mozilla

-Dorg.eclipse.swt.browser.UseWebKitGTK=true
-Dorg.eclipse.swt.browser.XULRunnerPath==/usr/lib/xulrunner-2
```
第一条是打开jsp文件闪退，第二、三条是打开class文件闪退，完全不是使用代码提示的原因。
但是问题就这样解决了，再也不会闪退啦
