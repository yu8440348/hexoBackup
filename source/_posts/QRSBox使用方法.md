title: QRSBox使用方法
tags:
  - Ubuntu
categories: []
date: 2015-01-07 22:03:00
---
#### 命令行工具使用方法

> 将hexo生成的文件上传到到七牛后便可以直接访问了

首先，下载 QRSBox 命令行工具，并解压。

然后，在解压后的文件夹中执行以下命令，进行初始化：

    ./qrsboxcli init <AccessKey> <SecretKey> <SyncDir> <Bucket> [<KeyPrefix>]
其中，**AccessKey**和 **SecretKey**在七牛云存储平台上申请。步骤如下：

开通七牛开发者帐号
登录七牛开发者自助平台，查看**Access Key** 和 **Secret Key**
**SyncDir**是本地的同步目录，该目录下的文件会随时同步上传值七牛云存储。

**Bucket**是保存同步文件的资源空间名。

**KeyPrefix**是文件前缀，可选。如果设置了该参数，那么上传的文件名前都会加上前缀。这个前缀主要用于在空间中区分不同上传来源的文件。

最后，用户可以使用以下命令开始文件同步：

	./qrsboxcli sync &
这里使用了 & 符号，让同步客户端进程运行在后台。如果退出终端后程序中断，请使用以下命令代替：

	nohup ./qrsboxcli sync >/dev/null 2>&1 &
用户可以通过以下命令查看同步过程：

	./qrsboxcli log
如果需要停止后台运行的**qrsboxcli**，可以使用如下命令：

	./qrsboxcli stop
如果希望改变同步的目录、bucket等运行参数，需要先用 stop 命令停止 qrsboxcli 的后台程序，重新用新的参数运行初始化命令，然后再次启动同步程序，qrsboxcli会立刻按新的配置将新目录的文件同步至七牛云存储。

#### 命令使用说明

执行以下命令可以获得各个子命令的使用说明：
```
./qrsboxcli
Usage:
  qrsboxcli init <AccessKey> <SecretKey> <SyncDir> <Bucket>  - Init qrsbox conf
  qrsboxcli sync &                                           - Watch <SyncDir> and sync files
  qrsboxcli log                                              - View sync log
  qrsboxcli stop                                             - Kill qrsboxcli sync process

BuildVersion:
  qrsboxcli v2.5.20131013
  ```
配置文件

命令行工具的配置文件通常保存在用户主目录的.qrsbox下，执行init命令时会将具体目录路径输出到屏幕上。 具体内容如下（JSON格式）：
```javascript
{
    "tasks": [
        {
            "src": "<SyncDir>",
            "dest": "qiniu:bucket=<Bucket>",
            "skipsym": 0,
            "syncdur": 0
        }
    ],
    "access_key": "<AccessKey>",
    "secret_key": "<SecretKey>",
    "debug_level": 0
}
```
其中，

- tasks字段指定监控任务：
- src字段指定受监控的文件目录；
- dest字段指定上传目标参数，如空间名（）和文件前缀（KeyPrefix），多个参数须以&符号分隔；
- skipsym字段指定是否跳过链接文件，0表示不跳过，1表示跳过；
- syncdur字段指定监控检测周期，单位为秒，0表示使用默认值（0.5秒）。
- access_key字段指定AccessKey值；
- secret_key字段指定SecretKey值；
- debug_level字段指定日志信息输出等级，默认值为0，即输出Debug信息。
ignore 文件与规则

> qrsbox 和 qrsboxcli 支持使用 ignore 文件来忽略某些不需要上传的文件，详见 ignore 规则。