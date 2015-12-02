title: MyEclipse10的正确破解方法
tags: Ubuntu
categories: []
date: 2015-01-07 16:41:00
---
本文转载自 http://blog.csdn.net/xexiyong/article/details/8273440
- MyEclipse10.1 破解工具下载http://download.csdn.net/detail/xexiyong/4862474或
http://download.csdn.net/detail/xexiyong/5448493，后者链接有自带的说明。这里说前者，但鄙人看后者效果更好些。  

首先确定你的 JDK以及环境变量没有问题！

 1、双击 run.bat打开破解界面  
 
 2、Usercode随便输入，点 SystemId按钮产生一个 SystemId，再点 Active按钮。下面会产生一些东西。  

 3、打开 MyEclipse，MyEclipse‐ Subscription Information，把上面生成的 LICENSEE复制到Subscripter 中，LICENSE_KEY复制到 Subscriptioncode中。就会有如下界面：  
 
 4、点 Save Activate Now按钮，弹出窗口，选择 Web activationconnect using your webbroswer，点 Next，产生一个 URL，URL中有个参数 SystemId，复制到破解界面的 SystemId中。  
 
 5、此时关闭 MyEclipse，按一下步骤操作破解界面： Tools‐‐‐rebuildKey Tools‐‐‐saveProperties Tools‐‐‐‐‐ReplaceJarFile‐‐‐‐‐选择你的安装目录/myeclipse/Common/plusgin目录 Tools‐‐‐saveProperties再次点击 Active按钮将会生成新的 ACTIVATION_KEY。

6、打开 MyEclipse， MyEclipse‐SubscriptionInformation，再次把上面生成的 LICENSEE 复制到 Subscripter 中，LICENSE_KEY 复制到 Subscriptioncode中。点击 ActivateNow按钮，选择 Ialreadyhaveanactivationcode。点 Next，把破解界面的 ACTIVATION_KEY复制到弹出的窗口中。

7、点 next，破解成功。
