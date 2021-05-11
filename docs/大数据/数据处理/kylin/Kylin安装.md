# Kylin部署

## 版本选型

```
3.1.2
```

## 安装

### 安装地址

官网地址：http://kylin.apache.org/cn/
官方文档：http://kylin.apache.org/cn/docs/
下载地址：http://kylin.apache.org/cn/download/

### 安装部署

1）下载并上传到/opt/software

```
https://www.apache.org/dyn/closer.cgi/kylin/apache-kylin-3.1.2/apache-kylin-3.1.2-bin-hadoop3.tar.gz
```

2）解压 apache-kylin-3.1.2-bin-hadoop3.tar.gz 到 /opt/module

```
tar -zxvf apache-kylin-3.1.2-bin-hadoop3.tar.gz -C /opt/module/

cd /opt/module
mv apache-kylin-3.1.2-bin-hadoop3 kylin
```

> `注意`：需要在/etc/profile文件中配置HADOOP_HOME，HIVE_HOME，HBASE_HOME并将其对应的 sbin（如果有这个目录的话）和bin目录配置到Path，最后需要source使其生效。

```
vim /etc/profile
```

```
#hadoop
export HADOOP_HOME=/opt/module/hadoop-3.1.3
export PATH=$PATH:$HADOOP_HOME/bin

#hive
export HIVE_HOME=/opt/module/hive
export HIVE_CONF=/opt/module/hive/conf
export PATH=$PATH:$HIVE_HOME/bin

#hbase
export HBASE_HOME=/opt/module/hbase-2.0.5
export PATH=$PATH:$HBASE_HOME/bin

#kylin
export KYLIN_HOME=/opt/module/kylin
export PATH=$PATH:$KYLIN_HOME/bin
```

```
source /etc/profile
```

3）启动

启动Kylin之前，需先启动Hadoop（hdfs, yarn, jobhistoryserver） 、Zookeeper、 Hbase

```
# 启动hadoop
hdp start

# 启动zk
zk.sh start

# 启动hbase
start-hbase.sh

# 启动k
kylin.sh start
```

启动之后查看各个节点进程：

```
xcall jps 
```

```
--------------------- tsingdata01 ----------------
3360 JobHistoryServer(MR的历史服务，必须启动)
31425 HMaster
3282 NodeManager
3026 DataNode
53283 Jps
2886 NameNode
44007 RunJar
2728 QuorumPeerMain
31566 HRegionServer
--------------------- tsingdata02 ----------------
5040 HMaster
2864 ResourceManager
9729 Jps
2657 QuorumPeerMain
4946 HRegionServer
2979 NodeManager
2727 DataNode
--------------------- tsingdata03 ----------------
4688 HRegionServer
2900 NodeManager
9848 Jps
2636 QuorumPeerMain
2700 DataNode
2815 SecondaryNameNode
```

> 注意：启动Kylin之前要保证HDFS，YARN，ZK，HBASE相关进程是正常运行的。

在http://tsingdata01:7070/kylin查看Web页面

```
用户名为：ADMIN
密码为：KYLIN（系统已填）
```

4）关闭

```
bin/kylin.sh stop
```

