title: HDFS java API增删改查操作
tags:
  - Hadoop
categories: []
date: 2015-01-16 15:09:00
---
### HDFS java API增删改查操作
运行此demo注意以下几点
> 1.hdfs安全模式需要关闭 命令：./hadoop dfsadmin -safemode leave  
> 2.引用jar包，需要将hadoop/share/hadoop/common/lib写的jar包都引用，并且还要引用hdfs目录下的hadoop-hdfs-2.6.0.jar  
> 3.hadoop集群用户权限的问题，以及各个目录的作用

#### 代码如下
```java
package demo;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.BlockLocation;
import org.apache.hadoop.fs.FSDataOutputStream;
import org.apache.hadoop.fs.FileStatus;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.hdfs.DistributedFileSystem;
import org.apache.hadoop.hdfs.protocol.DatanodeInfo;

public class FileManager {
	public static final String URL = "hdfs://localhost:9000";

	public static void main(String[] args) throws IllegalArgumentException,
			Exception {
		// CreateFile();
		// ReName();
		// CopyFile();
		// Mkdirs();
		// DeleteFile();
		// Exists();
		// ListFile("/");
		//FileLoc();
		GetHostName();
	}

	// 创建文件
	public static void CreateFile() throws IllegalArgumentException, Exception {
		FileSystem hdfs = getFileSystem();
		byte[] buff = "hello hadoop world!\n".getBytes();
		// 在test目录下创建hello文件
		Path dfs = new Path(URL + "/test/hello");
		FSDataOutputStream outputStream = hdfs.create(dfs);
		outputStream.write(buff, 0, buff.length);
	}

	private static FileSystem getFileSystem() throws Exception {
		Configuration conf = new Configuration();
		// 下面这行代码是为了解决路径问题，否则会提找不到路径，
		// 或者将hadoop目录中的conf文件夹中的hdfs-site.xml与core-site.xml复制到你的项目的目录之下，
		// 这样就不会再报这种错误
		conf.set("fs.default.name", "hdfs://localhost:9000");
		return FileSystem.get(conf);
	}

	// 重命名文件
	public static void ReName() throws Exception {
		FileSystem hdfs = getFileSystem();
		Path frpaht = new Path("hdfs://localhost:9000/test"); // 旧的文件名
		Path topath = new Path("hdfs://localhost:9000/test1"); // 新的文件名
		boolean isRename = hdfs.rename(frpaht, topath);
		String result = isRename ? "成功" : "失败";
		System.out.println("文件重命名结果为：" + result);
	}

	// 上传本地文件
	public static void CopyFile() throws Exception {
		FileSystem hdfs = getFileSystem();
		// 本地文件
		Path src = new Path("/home/yuhaitao/QiNiu.log");
		// HDFS地址
		Path dst = new Path(URL + "/test");
		hdfs.copyFromLocalFile(src, dst);
		System.out.println("Upload to" + URL);
		FileStatus files[] = hdfs.listStatus(dst);
		for (FileStatus file : files) {
			System.out.println(file.getPath());
		}
	}

	// 创建文件夹
	public static void Mkdirs() throws Exception {
		FileSystem hdfs = getFileSystem();
		Path dfs = new Path(URL + "/TestDir");
		hdfs.mkdirs(dfs);
		ListFile("/");
	}

	// 文件列表
	public static void ListFile(String path) throws Exception {
		FileSystem hdfs = getFileSystem();
		FileStatus files[] = hdfs.listStatus(new Path(URL + path));
		for (FileStatus file : files) {
			System.out.println(file.getPath() + " "
					+ file.getModificationTime());
		}
	}

	// 删除文件
	public static void DeleteFile() throws Exception {
		FileSystem hdfs = getFileSystem();
		Path delef = new Path(URL + "/TestDir");
		boolean isDeleted = hdfs.delete(delef, false);
		// 递归删除
		// boolean isDeleted=hdfs.delete(delef,true);
		System.out.println("Delete?" + isDeleted);
		ListFile("/");
	}

	// 文件是否存在
	public static void Exists() throws Exception {
		FileSystem hdfs = getFileSystem();
		Path findf = new Path(URL + "/test");
		boolean isExists = hdfs.exists(findf);
		System.out.println("Exist?" + isExists);
	}

	// 定文件在HDFS集群上的位置，其中file为文件的完整路径，start和len来标识查找文件的路径
	public static void FileLoc() throws Exception {
		FileSystem hdfs = getFileSystem();
		Path fpath = new Path(URL + "/test/hello");
		FileStatus filestatus = hdfs.getFileStatus(fpath);
		BlockLocation[] blkLocations = hdfs.getFileBlockLocations(filestatus,
				0, filestatus.getLen());
		int blockLen = blkLocations.length;
		for (int i = 0; i < blockLen; i++) {
			String[] hosts = blkLocations[i].getHosts();
			System.out.println("block_" + i + "_location:" + hosts[0]);
		}
	}

	//获取HDFS集群上的所有节点名称
	public static void GetHostName() throws Exception {
		FileSystem fs = getFileSystem();
		DistributedFileSystem hdfs = (DistributedFileSystem) fs;
		DatanodeInfo[] dataNodeStats = hdfs.getDataNodeStats();
		for (int i = 0; i < dataNodeStats.length; i++) {
			System.out.println("DataNode_" + i + "_Name:"
					+ dataNodeStats[i].getHostName());
		}
	}

}

```