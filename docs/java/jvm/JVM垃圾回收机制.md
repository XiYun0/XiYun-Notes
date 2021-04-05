# 垃圾回收

## GCRoots

Java内存分析工具MAT(Memory Analyzer Tool)：http://www.eclipse.org/mat/downloads.php

```r
BootStrap加载器加载的类对象
当前活动栈帧的局部变量（引用）
堆中被引用的对象

都可以作为GCRoot
```

## 引用

### 强引用

GCRoot直接引用的对象

### 软引用

GCRoot间接引用的对象，如果发生了垃圾回收，内存仍然不足时会对这一类对象进行回收。

### 弱引用

每发生一次垃圾回收，就会回收掉这种对象。

### 虚引用



## 垃圾回收算法

### 标记-清除

### 标记-复制

### 标记-整理

### 分代理论

## 垃圾回收器

