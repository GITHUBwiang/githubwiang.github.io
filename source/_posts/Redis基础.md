---
title: Redis基础
date: 2020-10-03 20:53:49
categories: learn
tags: redis
---

## Redis 简介

* Redis 是key-value型 NoSQL 数据库
* Redis 将数据存储在内存中，同时也能持久化到磁盘中
* Redis 常用于缓存，利用内存的高效提高程序的处理速度

## Redis 特定

* 速度快
* 广泛的语言支持
* 持久化
* 多种数据结构
* 主从复制
* 分布式与高可用

## Redis 常用配置

| 配置        | 示例               | 说明                  |
| ----------- | ------------------ | --------------------- |
| daemonize   | daemonize yes      | 是否启用后台运行      |
| port        | port 6379          | 设置端口号            |
| databases   | databases 16       | 设置 redis 数据库个数 |
| requirepass | requirepass abc123 | 设置 redis 连接密码   |

## Redis通用命令

* `select`
* `set`
* `get`
* `keys`
* `dbsize`
* `exists`
* `del`
* `expire`
* `ttl`

## Redis 数据结构

* `string`：字符串，值的长度不能超过512 MB。
  1. `set key value [EX seconds] [PX milliseconds] [NX|XX]`
     * `EX`：将键的过期时间设置为`seconds`秒，`set key value ex seconds`等同于`setex key seconds value`
     * `PX`：将键的过期时间设置为`milliseconds`毫秒，与`psetex key milliseconds value` 等价
     * `NX|XX`：只有在键不存在、存在时，才对键执行设置操作，`set key value NX`等同于`setnx key value`
  2. `mset key value [key value ...] mget key [key ...]`
  3. `incr key | incrby k increment`
  4. `decr key | decrby key decrement`
  5. `exists key [key ...]  del key [key ...]`
  6. `expire key seconds  ttl key `
* `hash`：键值类型
* `list`：列表，Redis lists基于Linked Lists实现。
* `set`：无序排列
* `zset`：有序排列

## Java 使用 Redis

