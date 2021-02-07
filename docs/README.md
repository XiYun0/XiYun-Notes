<p align="center">
  <a href="https://github.com/XiYun0/MultiAgentLearning "><img src="https://img.shields.io/badge/阅读-read-brightgreen.svg" alt="阅读"></a>
  <a href="#联系我"><img src="https://img.shields.io/badge/chat-微信群-blue.svg" alt="微信群"></a>
</p>


>github阅读：https://xiyun0.github.io/XiYun-Notes/#/
>
>gitee阅读：

>是故易有太极，是生两仪，两仪生四象，四象生八卦，八卦定吉凶，吉凶生大业。——《系辞》
>
>伏羲演八卦而可知万物，万物皆对象！



> 写下一个小心愿，成为一名架构师。

# 资源

1. [开源项目](others/开源项目.md)
2. [视频](others/视频.md)
3. [书单](others/书单.md)

# 注：

> 代码和绘图都放在了坚果云里面。

# Java

> 学好Java，走遍天下都不怕

## 基础

1. [Java 基础面试总结](java/base/Java基础.md)

2. [Java 反射](java/base/Java反射.md)

3. 注解

4. [Java 8 新特性总结](java/What's%20New%20in%20JDK8/Java8Tutorial.md)

   

## IO

1. [BIO,NIO,AIO 总结 ](java/BIO-NIO-AIO.md)

## 集合（容器）

> `集合`就是把一组拥有相同类型的变量保存起来的`容器`。

1. [Java集合面试总结](java/base/Java集合.md)

## JVM

>Java与C++之间有一堵由内存动态分配和垃圾收集技术所围成的“高墙”，墙外面的人想进去，墙里面的人却想出来。

书籍推荐：《深入理解Java虚拟机》、《自己动手写Java虚拟机》、《Java性能权威指南》

视频推荐：尚硅谷宋红康、黑马满一航

<img src="图片/170ec7cd31581d15" alt="img" style="zoom: 80%;" />

1. [Class Files 结构](java/jvm/类文件结构.md)
   1. 字符串常量池StringTable
2. 类加载器子系统
   1. [类加载过程](java/jvm/类加载过程.md)（广义的加载）
   2. [类加载器](java/jvm/类加载器.md)（狭义的加载）
3. [Java内存区域](java/jvm/Java内存区域.md)
4. 执行引擎
   1. 解释器
   2. 即时编译器
5. [JVM垃圾回收机制](java/jvm/JVM垃圾回收机制.md)
6. [JDK 监控和故障处理工具](java/jvm/JDK监控和故障处理工具总结.md)

## JUC

视频推荐：尚硅谷周阳、黑马满一航

1. [JMM](java/juc/JMM.md)
2. [Java锁](java/juc/Java锁.md)
   1. Lock
   2. AQS
   3. ReentranLock
   4. 读写锁
3. 并发容器
4. 工具类
5. 阻塞队列
6. 线程池

## 编程规范

1. [Java 编程规范](java/Java编程规范.md)

## Java-GUI

1. [JavaFx](java/gui/JavaFx.md)
2. Swing

## 源码阅读

1. [String源码](java/base/String源码.md)

# Spring框架

## SSM

- [Spring](system-design/framework/spring/Spring.md)
- SpringMVC
- Mybatis

## SpringBoot

- [SpringBoot基础](system-design/framework/spring/SpringBoot基础.md)
- [SpringBoot核心——Web](system-design/framework/spring/SpringBoot核心——Web.md)
- [SpringBoot核心——数据访问](system-design/framework/spring/SpringBoot核心——数据访问.md)
- [SpringBoot——Mybatis](system-design/framework/spring/SpringBoot——Mybatis.md)
- [文件上传](system-design/framework/spring/SpringBoot文件上传.md)
- 若依单模块前后端分离版

# 计算机基础

## 操作系统

- 虚拟化
  - 虚拟化CPU
  - [内存管理](operating-system/内存管理.md)
- [死锁](operating-system/死锁.md)
- 持久化

## 计算机网络

- [何为网络？](network/何为网络.md)

- [计算机网络面试题]()

## Linux

- 虚拟机Linux安装
- [后端程序员必备的 Linux 基础知识](operating-system/后端程序员必备的Linux基础知识.md)  
- [Shell 编程入门](operating-system/Shell.md) 

## 数据结构与算法

### 数据结构

- [数据结构知识学习与面试](dataStructures-algorithms/数据结构.md)

### 算法

- [算法学习资源推荐](dataStructures-algorithms/算法学习资源推荐.md)

- [刷爆力扣和剑指！](dataStructures-algorithms/刷爆力扣和剑指！.md)

# 数据库

## MySQL

1. [MySQL概述](database/MySQL.md)
2. 索引
3. 存储引擎

## Redis

1. [Redis概述 ](database/Redis.md)
2. [Redis安装](database/redis/Redis安装.md)
3. [Redis持久化及其意义](database/redis/Redis持久化.md)
4. [LRU缓存清除算法](database/redis/LRU缓存清除算法.md)
5. [缓存雪崩、缓存穿透](database/redis/缓存雪崩、缓存穿透.md)
6. [Redis的实战](database/redis/Redis实战.md)

## ElasticSearch

## Hbase



# 开发工具



1. lombok
2. jerbel——热部署



# 系统设计

## 设计模式

- [设计模式系列文章](system-design/设计模式.md)

## 高并发

### 缓存架构

>Redis——支撑高并发 



### 异步队列架构

### 分库分表

## 高可用

1. [什么是高可用架构](system-design/high-available/什么是高可用架构.md)

### hystrix

限流、熔断、降级

### Sentinel

## 分布式

1. 集群

## 微服务

> 微服务：每个项目都是独立自治的

### 分布式组件

1. [SpringCloud](system-design/micro-service/SpringCloud.md)
2. [Nacos](system-design/micro-service/Nacos.md)-服务注册中心、配置中心
3. Sentinel-取代hystrix
4. [Gateway](system-design/micro-service/Gateway.md)-网关

#### ZooKeeper

- [ZooKeeper 相关概念总结](system-design/framework/ZooKeeper.md)

- [ZooKeeper 数据模型和常见命令](system-design/framework/ZooKeeper数据模型和常见命令.md)

## Docker

1. [Docker](system-design/docker/Docker.md)

# 大数据

## Hadoop

> 大数据存储引擎

## Flink

## Spark

## Flume

# Golang

- [Golang基础](golang/Golang基础.md)

# Python

- [功能齐全、强大的第三方库](python/第三方库.md)
- [OpenCV](python/OpenCV.md)
- [python脚本控制手机软件！尽搞些花里胡哨的](python/python脚本控制手机软件.md)
- [解放双手的自动化](python/自动化.md)
- [自动抢货](python/自动抢货.md)
- [游戏脚本](python/游戏脚本.md)
- [数据可视化](python/数据可视化.md)

# 人工智能

## 深度学习

### Pytorch

1. [pytorch笔记](ai/deeplearning/pytorch/pytorch笔记.md)
2. [张量Tensor](ai/deeplearning/pytorch/张量Tensor.md)

## 强化学习

1. 什么是强化学习

### 多智能体强化学习

> 这部分笔记在下面这个仓库
>
> - [多智能体系统学习指南](https://xiyun0.github.io/MultiAgentLearning/#/)

# 其他

1. [其他](others.md)