---
title: 2022-03~04 面试记录
excerpt: 社招的几次笔试面试记录。
date: 2022-03-01 11:18:42
tags: [面试, Java, Redis, MySQL]
categories: interview
---

# 易诚互动

## 一面

### 项目相关

* 目前项目某个功能的数据流向、代码如何做到可扩展；
* MyBatis 的工作原理；
* 如何实现文件上传、下载显示实时进度。

## 二面

### 多线程

* 线程池的使用和基础参数；
* 什么是线程上下文。

### Redis

* Redis 的特点、为什么快；
* Redis 的使用场景；
* Redis 的缓存雪崩和应对方法；
* Redis 的持久化方式。

### MySQL

* 如何优化 SQL 语句；
* 索引失效的情况；
* 如何降低 MySQL 的压力。

### MQ

* 如何处理消息的重复发送和消息丢失。

### JVM

* JVM 调优经验；
* 对象如何进入老年代。

### 分布式

* 项目中用了何种分布式框架、什么场景；
* 对 Dubbo 的掌握和理解。

## 结果

13K * 13



# 易方达基金

## 笔试

### 选择题

涉及计算机网络、数据结构、Java 基础、项目管理等。

### 简述题

* 在浏览器中键入域名到显示页面的过程；
* 简述 ClassLoader 的作用和工作原理；
* Java 线程中有哪些同步和交互的机制；
* 忘了。

### 算法题

（在线编辑器，允许切屏使用本地 IDE）

* 给定一个未经排序的整数数组，请使用 Java 语言编码实现找到最长且连续的的递增序列的长度。输入: [1,3,5,4,7] 输出: 3；输入: [2,2,2,2,2],输出: 1；
* 字符串压缩，形如 aaaa -> a4, AAbbbdf -> A2b3df；

### 系统设计题

* 如何设计实现一个支持弹性伸缩和 7*24H 服务的系统。

### 编码题

（纯文本编辑器，没有语法提示）

* 使用 ForkJoin 框架实现计算 1~10000 的和。

## 一面

### 项目相关

* 介绍下目前正在做的项目；
* 平时项目开发的具体流程；
* 怎么排查生产问题；
* 为新项目搭建环境，如何做技术选型？你会选择 Redis 还是 MongoDB？

### Java 相关

* Java 中抽象类和接口的比较；
* Java 字符串和字符数组、字节数组的转换 `toCharArray`、`getBytes`。

### 字符集

* 有哪些字符集；
* ISO-8859-1 的了解。

### 设计模式

* 有哪些常用的设计模式，在哪里使用或看见过；
* 介绍观察者模式。

### 数据库

* 对 MySQL 的数据同步的了解（binlog、canal）。

### MQ

* 消息队列中消费者是否需要对消息进行确认；
* 对未确认的消息，队列怎么处理？消费者还能继续查看消费到它吗？
* 未对消息确认前业务节点异常宕机，该消息怎么处理？

### Spring、MyBatis

* 谈谈自己对 Spring 的了解和理解；
* Spring 中怎么对 MyBatis 做事务管理；
* Spring 中定义的事务传播行为，默认传播行为是什么？

### JVM

* 栈内存会溢出吗？
* 怎么排查 OOM 问题。

### 分布式相关

* 有哪些 RPC 协议（RPC是上层协议，底层可以基于 TCP 协议，也可以基于 HTTP 协议）
* 多个系统如何查看日志（分布式监控）；
* 阿里云的鹰眼如何实现的？日志中 `[-JYLSH-null-]` 是如何实现的？
* 你如何实现鹰眼的链路追踪功能？
* 如何处理分布式事务。

## 二面

### 闲聊

* 目前项目情况；
* 项目中遇到的困难的问题，如何排查解决的；
* 最近在学习什么东西；
* 你觉得自己的技术水平怎么样？
* 你期望的工资是多少？

当时说我不到三年的工作经验，价格可能要高了，所以安排我多做了一轮笔试题和面试题。

## 笔试题（加餐）

### 选择题

涉及计算机网络、数据结构、操作系统、Java 基础、项目管理等。

### 简述题

* Java 的并发包（JUC）中，除了提供高性能的线程安全集合类，还提供了很多并发场景需要的原子类，请列举你熟悉的并发包类和使用场景；
* Java 线程中有哪些同步和交互的机制；
* 在分布式架构下，如何在不影响客户请求的情况下做到弹性扩展和 7*24 小时服务可用，同时如何识别同一个客户在不同服务器的请求，请简述你的方案；
* 列出 Java 发生内存溢出的几种不同场景和发生原因。

### 编码题

（纯文本编辑器，没有语法提示）

* 在一个大小为 N 的整形数组中，寻找第 K 大的数。要求不能对数组全部排序；
* 字符串压缩，形如 aaaa -> a4, AAbbbdf -> A2b3df；
* 问题描述：编写一个程序，模拟一个100米短跑的场景。每个运动员的跑步过程就是一个线程。
  要求如下：
  1） 发令枪鸣枪后选手们开始赛跑；
  2） 在所有运动员达到终点后系统开始统计选手们的成绩并把成绩发送到成绩统计系统；
  3） 对所有运动员的成绩进行排序输出。

## 面试（加餐）

### 技术

* JVM 内存划分及内存溢出情况；
* 不断创建线程会发生什么；
* 如何避免 SQL 注入；
* 如何获取用户的物理地址；
* restful 接口。

## 项目

* 目前团队的组成和分工；
* 举一个最近做的功能，如何对其分解设计和编码实现；
* 举一个排查问题的经历；
* 谈谈自己的编码规范，列举几个；
* 怎么做对外接口设计的，有哪些重要的点。

## 结果

17 * 14



# ByteGreen

## 性格测试

好像都可以随便选，也没看到后续有看这个，直接就叫去面试了。

## 面试

### 项目相关

* xxl-job 在项目中是如何工作的；
* 目前有在学习哪方面的内容。

### Java

* 方法重载和方法重写的区别；
* Java 方法是如何调用的，如何正确选择多个重载的方法进行调用；
* `protected` 关键字的作用；
* Java 优先级队列：`proviteQueue`；
* 静态代码块、静态变量和构造函数的执行顺序是什么，为什么是这个执行顺序。

### JVM

* 判断垃圾对象的方法；

* 常见垃圾收集器的算法和实现。

### Redis

* Redis 的缓存穿透和解决方法；
* Redis 的缓存雪崩和解决方法。

### MQ

* 如何做到消息不丢失？如何判断消息已成功发送。

### Spring

* 谈谈对 Spring 中 IOC 和 APO 的理解，其对应实现原理；

### 计算机基础、网络基础

* 有没有接触过物联网操作系统；
* 浏览器输入地址到页面展现内容的过程，请结合计算机硬件和软件相关知识进行描述；
* 网际层 IP 协议如何保证收到的包是完整的。

## 复试

我说太远了，能不能调整到周末去参加复试。HR 协商后说周末没有时间，就不用参加复试了。

## 结果

19 * 13



# 太平洋网络科技

## 一面

太难了，面试官一半的时间都在说：“这个不会没关系，我们换一个”，大致涉及如下部分：

### 数据结构和算法

* 红黑树、二叉树；
* 二叉树怎么用于查找；
* 有向图的最短路径和算法。

### 缓存

* Redis 和 MemCache 的区别；
* MemCache 如何实现集群。

### 数据库

* MongoDB 的了解；
* MySQL 如何分库分表，数据迁移是怎么做的。

### SpringCloud

* SpringCloud 怎么做负载均衡的，有哪些策略。

### MQ

* 消息如何做到分布式一致性；
* 如何确认消息发送成功。

### ElasticSearch

* 有没有用过 Elasticsearch。

## 结果

没有结果就是最好的结果。



# 广州邮政

比较简单，就感觉像是招应届毕业生一样。

## 一面

### Java 基础

* 创建 String 对象的几种方式；
* ArrayList 和 LinkedList 的特点和使用场景；
* List 和 Set 的区别；
* Map 的实现类和使用场景。

### MySQL

* 如何做分库分表，如何迁移数据；
* 如何做 SQL 优化。

### Linux

* 线上排查问题时常用哪些指令；
* 随便说说你知道的 Linux 指令。

## 结果

14K * 13



# 华为外包

## 算法题

* 字符串 aaabbccccd 压缩后为 3abb4cd，请编写解压函数，根据输入的字符串（只包含大小写字母和数字），判断是否为合法压缩的字符串，若合法则输出解压缩后的字符串，否则输出 !error。

  ```java
  4dff -> ddddf
  
  2dff -> !error
  
  4d@A ->!error
  ```

* 给定 N 个正整数，为每个正整数图上一种颜色。要求同种颜色的所有数都可以被这种蓝色中最小的那个数整数。算出最少需要多少种颜色才能给这 N 个数进行上色。

  输入描述：

  第一行输入一个正胜数

  第二行有 N 个 int 型数

  ```java
  3
  
  2 4 6
  
  1
  
  
  
  4
  
  2 3 4 9
  
  2
  ```

* 牌面由颜色和数字组成，颜色为红、黄、蓝、绿中的一种，数字为0-9中的一个。开始时从手牌中选取一张卡牌打出，接下来如果玩家中有他上一次打出手牌的颜色或着数字相同的手牌，则可以继续打出，直至手牌打光或者没有符合条件的手牌，请找出最大可连续出牌的次数。

  ```java
  1 4 3 4 5
  
  r y b b r
  
  3 （4y 4b 3b）
  
  1 2 3 4
  
  r y b l
  
  1

## 结果

确定去 ByteGreen 所以没有去参加面试。
