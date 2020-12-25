---
title: ThreadLocal
date: 2020-11-30 20:46:36
categories: learn
tags: java
---

# ThreadLocal

## ThreadLocal 典型应用场景

### 线程独享对象

* 每个 Thread 内有自己的实例副本，各个线程间不共享。

### 线程保存全局变量

* 避免参数在各个方法之间传递

## ThreadLocal 主要用法

### `initialValue`

* 返回当前线程对应的“初始值”，是一个延迟加载的方法，只有在调用`get`的时候，才会触发
* 一个线程只需要调用一次此方法，可以调用`remove`方法删除后在调用`initialValue`

### `set`

* 为这个线程设置一个新值

### `get`

* 获取保存的值，如果为空，则调用`initialValue`方法初始化

### `remove`

* 移除当前值

## ThreadLocal 基本原理

## ThreadLocal 使用的注意点

### 内存泄漏

* 某个对象不再被使用，但占用的内存无法被回收。
* 主动调用`remove`方法避免内存泄漏