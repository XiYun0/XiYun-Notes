# HDFS

## 概述

HDFS产生背景

```r
随着数据量越来越大，在一一个操作系统存不下所有的数据，那么就分配到更多的操作系统管理的磁盘中，但是不方便管理和维护，迫切需要一种系统来管理多台机器上的文件，这就是分布式文件管理系统。HDFS 只是分布式文件管理系统中的一种。
```

HDFS定义

```r
HDFS (Hadoop Distributed File System)，它是一个文件系统，用于存储文件，通过目录树来定位文件；其次，它是分布式的，由很多服务器联合起来实现其功能，集群中的服务器有各自的角色。
HDFS的使用场景：适合一次写入，多次读出的场景。一个文件经过创建、写入和关闭之后就不需要改变。
```



## API

```java
@Test
public void testCopyFromLocalFile() throws IOException, InterruptedException, URISyntaxException {
        // 1 获取文件系统
        Configuration configuration = new Configuration();
        configuration.set("dfs.replication", "2");
        FileSystem fs = FileSystem.get(new URI("hdfs://hadoop102:8020"),
        configuration, "atguigu");
        // 2 上传文件
        fs.copyFromLocalFile(new Path("d:/sunwukong.txt"), new
        Path("/xiyou/huaguoshan"));
        // 3 关闭资源
        fs.close();
}
```





## HDFS 的读写流程



# 参考

尚硅谷hadoop