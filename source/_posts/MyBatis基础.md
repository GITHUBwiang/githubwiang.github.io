---
title: MyBatis基础
date: 2020-09-15 21:41:19
categories: learn
tags: mybatis
---

# MyBatis 基础

* [MyBatis 中文文档](https://mybatis.org/mybatis-3/zh/index.html)
* [MyBatis-Spring 中文文档](http://mybatis.org/spring/zh/)
* [MyBatis-Spring-Boot 文档](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)
* [PageHelper 地址](https://github.com/pagehelper/Mybatis-PageHelper)

## MyBatis 介绍

* MyBatis 是优秀的持久层框架
* MyBatis 使用 XML 将 SQL 与程序代码解耦，便于维护
* MyBatis 学习简单，执行高效，是 JDBC 的延伸

## MyBatis 开发流程

1. 引入 MyBatis 依赖
2. 创建核心配置文件
3. 创建实体（Entity）类
4. 创建 Mapper 映射文件
5. 初始化 SessionFactory
6. 利用 SQLSession对象操作数据

## SqlSessionFactory

* SqlSessionFactory 是 MyBatis 的核心对象
* SQLSessionFactory 用于初始化MyBatis，创建 SQLSession 对象
* 应保证 SQLSessionFactory 全局唯一

## SqlSession

* SQLSession 是MyBatis 操作数据库的核心对象
* SQLSession 使用 JDBC 与数据库交互

## MyBatis 查询

## MyBatis 日志框架

Mybatis 通过使用内置的日志工厂`org.apache.ibatis.logging.LogFactory`提供日志功能。内置日志工厂将会把日志工作委托给下面的实现之一：

* `org.apache.ibatis.logging.slf4j.Slf4jImpl`
* `org.apache.ibatis.logging.commons.JakartaCommonsLoggingImpl`
* `org.apache.ibatis.logging.log4j.Log4jImpl`
* `org.apache.ibatis.logging.log4j2.Log4j2Impl`
* `org.apache.ibatis.logging.jdk14.Jdk14LoggingImpl`
* `org.apache.ibatis.logging.nologging.NoLoggingImpl`

MyBatis 内置日志工厂会基于运行时检测信息选择日志委托实现。它会（按上面罗列的顺序）使用第一个查找到的实现。当没有找到这些实现时，将会禁用日志功能。

* MyBatis 查找日志的顺序定义在`org.apache.ibatis.logging.LogFactory`中

  ```java
  // 使用静态块加载日志框架
  static {
      tryImplementation(LogFactory::useSlf4jLogging);
      tryImplementation(LogFactory::useCommonsLogging);
      tryImplementation(LogFactory::useLog4J2Logging);
      tryImplementation(LogFactory::useLog4JLogging);
      tryImplementation(LogFactory::useJdkLogging);
      tryImplementation(LogFactory::useNoLogging);
  }
  
  public static synchronized void useSlf4jLogging() {
      setImplementation(org.apache.ibatis.logging.slf4j.Slf4jImpl.class);
  }
  ```

  

*  `NoLoggingImpl`中的所有方法都未实现，即为禁用日志功能。

```java
public class NoLoggingImpl implements Log {

  public NoLoggingImpl(String clazz) {
    // Do Nothing
  }
  
    @Override
  public void error(String s, Throwable e) {
    // Do Nothing
  }
}
```

* 可以在MyBatis 配置文件`mybatis-config.xml` 中使用`setting`标签来指定 MyBatis 的日志实现

  ```xml
  <configuration>
    <settings>
      ...
      <setting name="logImpl" value="LOG4J"/>
      ...
    </settings>
  </configuration>
  ```

  `logImpl` 的可选值有：`SLF4J`、`LOG4J`、`LOG4J2`、`JDK_LOGGING`、`COMMONS_LOGGING`、`STDOUT_LOGGING`、`NO_LOGGING`，或者是实现了`org.apache.ibatis.logging.Log`接口且构造方法以字符串为参数的类完全限定名。

  ```java
   private static void setImplementation(Class<? extends Log> implClass) {
      try {
        Constructor<? extends Log> candidate = implClass.getConstructor(String.class);
        Log log = candidate.newInstance(LogFactory.class.getName());
        if (log.isDebugEnabled()) {
          log.debug("Logging initialized using '" + implClass + "' adapter.");
        }
        logConstructor = candidate;
      } catch (Throwable t) {
        throw new LogException("Error setting Log implementation.  Cause: " + t, t);
      }
    }
  ```

## MyBatis 动态 SQL

### `if`



### `choose、when、otherwise`

### `trim、where、set`

### `foreach`



