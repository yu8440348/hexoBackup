title: FreeMarker遍历HashMap中对象
tags:
  - Java开发
categories: []
date: 2015-01-09 15:25:00
---
#### 在jfinal中使用FreeMarker遍历HashMap中对象
java代码如下
``` java
HashMap<String, FileModel> hashmap = new  HashMap<String,FileModel>();  
FileModel model = new FileModel();
model.setKey(item.key);
model.setHash(item.hash);
model.setFsize(item.fsize);
model.setMimeType(item.mimeType);
model.setPutTime(item.putTime);
hashmap.put(item.key, model);
```
FreeMarker代码如下
```html
<#if fileList?exists>
 	<#list fileList.keySet() as key>
       ${key}<br/> 这里是hashmap中的键值
       ${fileList[key].key}<br/>这里取对象的属性
 	</#list>
</#if>
```
