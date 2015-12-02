title: github绑定域名
date: 2015-01-05 16:41:54
tags: 日志
---
### GitHub绑定域名

1. 首先在项目根目录新建一个 CHAME文件，文件内容为 kuailelai.com

2. 然后在终端中,记录IP地址
``` bash
    ping yu8440348.github.io 
    PING github.map.fastly.net (103.245.222.133) 56(84) bytes of data.
    64 bytes from 103.245.222.133: icmp_seq=2 ttl=49 time=133 ms
```
3. 然后在dnspod中新加一条a记录，ip地址为上面的，然后在新加一条条chame记录，指向yu8440348.github.io 

4. 等待生效


