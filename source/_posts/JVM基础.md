---
title: JVM基础
excerpt: JVM 基础知识和一些面试题。
date: 2021-01-17 14:55:05
tags: jvm
categories: interview
index_img: https://raw.githubusercontent.com/xianglin2020/gallery/master/202102/JVM.png
---

# JVM 基础

## Java 基础知识

## 谈谈对 Java 的理解

* 平台无关性
* GC
* 语言特性：泛型、反射、Lambda
* 面向对象：封装、继承、多态
* 类库：网络、IO、集合、多线程
* 异常处理

### Java 的平台无关性是如何实现的

`javac` 命令将 Java 源程序编译成平台无关的字节码文件（.class），`java` 命令执行 Java 程序时通过不同平台上的 JVM 解析成机器指令。

Java 平台无关性是建立在 Java 虚拟机的平台有关性基础之上的，是因为 Java 虚拟机屏蔽了底层操作系统和硬件的差异。

### Java 常用命令行工具



### Java 反射基础用法

反射是在运行时，非编译时，动态获取类型的信息，比如接口信息、成员信息、方法信息、构造器信息等，根据这些动态获取到的信息创建对象、访问/修改成员、调用方法的高级语言特性。



## JVM 架构

![image-20210117152159865](https://raw.githubusercontent.com/xianglin2020/gallery/master/202101/image-20210117152159865.png)

### ClassLoader

* 启动类加载器（Bootstrap ClassLoader）：这个类加载器是 Java 虚拟机实现的一部分，不是 Java 语言实现的，一般是 C++ 实现的，它负责加载 Java 的基础类，主要是 `JAVA_HOME/lib/rt.jar`。
* 扩展类加载器（Extension ClassLoader）：负责加载 Java 的一些扩展类，一般是 `JAVA_HOME/lib/ext`目录中的 jar 包。
* 应用程序类加载器（Application ClassLoader）：负责加载应用程序的类，包括自己写的和引入的第三方类库，即所有在类路径中指定的类。

### 类的装载过程

![类的装载过程](https://github.com/xianglin2020/gallery/blob/master/202101/143838.jpg?raw=true)

1. 加载：通过 ClassLoader 加载 class 文件字节码，生成 Class 对象。
   1. 通过全类名获取定义此类的二进制字节流。
   2. 将字节流所代表的静态存储结构转换为方法区的运行时数据结构。
   3. 在内存中生成一个代表该类的 Class 对象，作为方法区这些数据结构的入口。
2. 连接：
   1. 验证：检查加载的 class 正确性和安全性。
   2. 准备：为类变量分配存储空间并设置类变量初始值。
   3. 解析：JVM 将常量池的符号引用转化为直接引用。
3. 初始化：执行类变量赋值和静态代码块。

### 双亲委派机制

![双亲委派机制](https://raw.githubusercontent.com/xianglin2020/gallery/master/202101/image-20210117192054967.png)

类加载过程：

1. 判断是否已经加载过，加载过了直接返回 Class 对象，一个类只会被一个 ClassLoader 加载一次；
2. 如果没有被加载，先让父 ClassLoader 去加载，如果加载成功，返回得到的 Class 对象；
3. 在父 ClassLoader 没有加载成功的前提下，自己尝试加载类。

双亲委派模型的好处：

* 避免多份同样字节码的加载。
* 保证 Java 核心 API 不被篡改。

### Java 运行时数据区

线程私有部分：

* 程序计数器：
  * 字节码解释器通过改变程序计数器来依次读取指令，实现代码的流程控制。
  * 程序计数器用于记录当前线程执行的位置，方便线程切换后继续执行。
* Java 虚拟机栈：
  * 描述 Java 方法执行的内存模型，每次方法调用的数据都是通过栈传递的。
  * Java 虚拟机栈包括：局部变量表、操作数栈、动态链接、方法出口信息。
* 本地方法栈为本地方法服务。
* 方法区：元空间（MetaSpace）使用本地内存，永久代（PermGen）使用的是 JVM 内存。
* 堆：
  * 对象实例的分配区域。
  * GC 管理的主要区域。

### JVM 性能调优参数的含义

* `-Xss`：规定了每个线程虚拟机栈的大小
* `-Xms`：堆的初始值
* `-Xmx`：堆能达到的最大值

### 堆和栈的区别

内存分配策略：静态存储、栈式存储、堆式存储。

引用对象、数组时，栈里定义变量保存在堆中目标的首地址。

* 管理方式：栈自动释放，堆需要 GC。
* 空间大小：栈空间比堆空间小。
* 碎片相关：栈产生的碎片远小于堆。
* 分配方式：栈支持静态分配和动态分配，而堆空间仅支持动态分配。
* 效率：栈空间效率比堆空间高。

### String 的 intern() 方法在 JDK6 和以后的区别

**JDK7 字符串常量池从永久代移动到堆中。**

### TODO