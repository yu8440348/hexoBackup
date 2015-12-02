title: hexo添加自定义挂件
date: 2015-01-06 23:40:57
tags: hexo
---

### 添加自定义挂件

- 在主题文件夹layout>_widget添加文件，如times.ejs  

- 然后在主题 _config.yml 配置如下

```
#### Widgets
widgets: 
- category
- tag
- tagcloud
- times #此处添加你上一步中添加的文件名
```

- 然后在times.ejs 添加自己的代码
