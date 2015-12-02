title: Ubuntu下安装深度音乐
date: 2015-01-06 13:28:00
tags: Ubuntu
---
#### Ubuntu下安装深度音乐

**通过PPA 进行安装DMusic深度音乐播放器**
```bash
sudo add-apt-repository ppa:noobslab/deepin-sc
sudo apt-get update
sudo apt-get install deepin-music-player
```
**然后我们先打开一下深度音乐，这样它会创建好文件夹方便安装百度音乐插件**  
接着：  

1.安装编译时的相关依赖包(cython libwebkitgtk-dev python-dev git),  

    sudo apt-get install cython libwebkitgtk-dev git
    
2.安装pyjavascriptcore
```bash
　　git clone https://github.com/sumary/pyjavascriptcore.git
　　cd pyjavascriptcore
　　sudo python setup.py install
```
3.安装百度音乐插件
```bash
　　git clone https://github.com/sumary/dmusic-plugin-baidumusic.git
　　cd dmusic-plugin-baidumusic
　　cp -r baidumusic ~/.local/share/deepin-music-player/plugins/
```
最后运行深度音乐， 选项设置->附加组件 中启用百度音乐即可。

4.深度影音播放器
我们可以通过PPA安装：
```bash
sudo add-apt-repository ppa:noobslab/deepin-sc
sudo apt-get update
sudo apt-get install deepin-media-player
```