title: hadoop问题Type mismatch in value from map解决方法
tags:
  - Hadoop
categories: []
date: 2015-01-21 18:50:00
---
``` java
12/08/27 15:49:40 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
12/08/27 15:49:40 WARN mapred.JobClient: No job jar file set.  User classes may not be found. See JobConf(Class) or JobConf#setJar(String).
12/08/27 15:49:41 INFO input.FileInputFormat: Total input paths to process : 4
12/08/27 15:49:41 INFO mapred.JobClient: Running job: job_local_0001
12/08/27 15:49:41 INFO util.ProcessTree: setsid exited with exit code 0
12/08/27 15:49:41 INFO mapred.Task:  Using ResourceCalculatorPlugin : org.apache.hadoop.util.LinuxResourceCalculatorPlugin@3249256e
12/08/27 15:49:41 INFO mapred.MapTask: io.sort.mb = 100
12/08/27 15:49:41 INFO mapred.MapTask: data buffer = 79691776/99614720
12/08/27 15:49:41 INFO mapred.MapTask: record buffer = 262144/327680
12/08/27 15:49:41 WARN mapred.LocalJobRunner: job_local_0001
java.io.IOException: Type mismatch in value from map: expected org.apache.hadoop.io.IntWritable, recieved org.apache.hadoop.io.Text
    at org.apache.hadoop.mapred.MapTask$MapOutputBuffer.collect(MapTask.java:1019)
    at org.apache.hadoop.mapred.MapTask$NewOutputCollector.write(MapTask.java:691)
    at org.apache.hadoop.mapreduce.TaskInputOutputContext.write(TaskInputOutputContext.java:80)
    at SmallFilesToSequenceFileConverter$SequenceFileMapper.map(SmallFilesToSequenceFileConverter.java:38)
    at SmallFilesToSequenceFileConverter$SequenceFileMapper.map(SmallFilesToSequenceFileConverter.java:1)
    at org.apache.hadoop.mapreduce.Mapper.run(Mapper.java:144)
    at org.apache.hadoop.mapred.MapTask.runNewMapper(MapTask.java:764)
    at org.apache.hadoop.mapred.MapTask.run(MapTask.java:370)
    at org.apache.hadoop.mapred.LocalJobRunner$Job.run(LocalJobRunner.java:212)
```
解决方法：

首先你看一下你map的输出和reduce的输入是不是对应的，然后看看你的map和reduce里的参数和下面的是不是设置的一样。
```java
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
```
> 转载自 http://blog.csdn.net/zjml2412/article/details/7913332
