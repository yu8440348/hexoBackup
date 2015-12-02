title: Eclipse智能提示及快捷键
tags:
  - Java开发
categories:
  - 编程相关
date: 2015-01-09 09:50:00
---
## java智能提示
(1). 打开Eclipse，选择打开" Window － Preferences"。

(2). 在目录树上选择"Java－Editor－Content Assist"，在右侧的"Auto-Activation"找到"Auto Activation triggers for java"选项。默认触发代码提示的就是"."这个符号。

(3). 在"Auto Activation triggers for java"选项中，将"."更改：.abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
<!--more-->
## XML智能提示
(1). 打开Eclipse，选择打开" Window － Preferences"。

(2). 在目录树上选择"XML－Editor－Content Assist"，在右侧的"Auto-Activation"找到"Prompt when these characters are inserted "选项。

(3). 在"Prompt when these characters are inserted"选项中，将"<=: ，"更改：<=:.abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVW(,

## 快捷键
(1)Ctrl+Space

说明:内容助理。提供对方法,变量,参数,javadoc等得提示,应运在多种场合,总之需要提示的时候可先按此快捷键。注:避免输入法的切换设置与此设置冲突

(2)Ctrl+Shift+Space

说明:变量提示

(3)Ctrl+/

说明:添加/消除//注释,在eclipse2.0中,消除注释为Ctrl+\

(4)Ctrl+Shift+/

说明:添加/* */注释

(5)Ctrl+Shift+\

说明:消除/* */注释

(6)Ctrl+Shift+F

说明:自动格式化代码

(7)Ctrl+1

说明:批量修改源代码中的变量名,此外还可用在catch块上.

(8)Ctril+F6

说明:界面切换

(9)Ctril+Shift+M

说明:查找所需要得包

(10)Ctril+Shift+O

说明:自动引入所需要得包

(11)Ctrl+Alt+S

说明:源代码得快捷菜单。其中的Generate getters and setters 和 Surround with try/catchblock比较常用.建议把它们添加为快捷键.快捷键设置在windows->preferences->Workbench->Keys

## 跟踪调式
单步返回 F7

单步跳过 F6

单步跳入 F5

单步跳入选择 Ctrl+F5

调试上次启动 F11

继续 F8

使用过滤器单步执行 Shift+F5

添加/去除断点 Ctrl+Shift+B

显示 Ctrl+D

运行上次启动 Ctrl+F11

运行至行 Ctrl+R

Ctrl+U重构作用域 功能 快捷键

撤销重构 Alt+Shift+Z

抽取方法 Alt+Shift+M

抽取局部变量 Alt+Shift+L

内联 Alt+Shift+I

移动 Alt+Shift+V

重命名 Alt+Shift+R

重做 Alt+Shift+Y
