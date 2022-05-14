---
title: Play2 in Java
date: 2022-04-18 21:18:35
tags: play2
categories: learn
excerpt: Play is a high-productivity Java and Scala web application framework that integrates the components and APIs you need for modern web application development.
---

# Getting started

官方网站：

[play](https://www.playframework.com/)

[sbt](https://www.scala-sbt.org/)

## 创建 Play2 项目

*你可能需要一个梯子🪜*

### 使用 IDEA 创建 Play2 项目

参见：[Getting started with Play 2.x](https://www.jetbrains.com/help/idea/getting-started-with-play-2-x.html)。

启动 Scala 插件：

![image-20220420212825206](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/202825.png)

通过 IDEA 的模板创建项目：

![image-20220418212421717](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/182421.png)

创建完成后 Play2 项目结构如下：

<img src="https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/182609.png" alt="image-20220418212609419" style="zoom:50%;" />

### 使用 sbt 从命令行创建

参见：[Creating a New Application](https://www.playframework.com/documentation/2.8.x/NewApplication)。

使用如下命令创建 Java Play 项目：

```shell
sbt new playframework/play-java-seed.g8
```

![image-20220420213629111](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/203629.png)

创建完成后的结构如下：

![image-20220420213903281](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/203903.png)

从 IDEA 中启动项目或者在控制台中输入`sbt run` 启动项目后访问 `http:localhost:9000`：

![image-20220418213906167](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/183906.png)

## 项目结构介绍

参见：[Anatomy of a Play application](https://www.playframework.com/documentation/2.8.x/Anatomy)。

```
app                      → Application sources
 └ assets                → Compiled asset sources
    └ stylesheets        → Typically LESS CSS sources
    └ javascripts        → Typically CoffeeScript sources
 └ controllers           → Application controllers
 └ models                → Application business layer
 └ views                 → Templates
build.sbt                → Application build script
conf                     → Configurations files and other non-compiled resources (on classpath)
 └ application.conf      → Main configuration file
 └ routes                → Routes definition
 └ logback.xml           → logback definition
dist                     → Arbitrary files to be included in your projects distribution
public                   → Public assets
 └ stylesheets           → CSS files
 └ javascripts           → Javascript files
 └ images                → Image files
project                  → sbt configuration files
 └ build.properties      → Marker for sbt project
 └ plugins.sbt           → sbt plugins including the declaration for Play itself
lib                      → Unmanaged libraries dependencies
logs                     → Logs folder
 └ application.log       → Default log file
target                   → Generated stuff
 └ resolution-cache      → Info about dependencies
 └ scala-2.13
    └ api                → Generated API docs
    └ classes            → Compiled class files
    └ routes             → Sources generated from routes
    └ twirl              → Sources generated from templates
 └ universal             → Application packaging
 └ web                   → Compiled web assets
test                     → source folder for unit or functional tests
```

### `app/`

`app` 目录包含所有可执行文件：Java 和 Scala 的源代码，模板文件和 `assets`；

默认创建三个包对应 MVC 的三层架构：

`app/controllers`

`app/models`

`app/views`

`app` 下可以随意创建包，包路径也是随意的，比如：`app/store/xianglin/play2/controllers` 或者 `app/store/xiangln/play2/services`。

### `public/`

`public` 存储资源文件，三个子目录分别用于存储 images、CSS 和 JavaScript。

### `conf/`

`conf` 目录存放 Play2 的配置文件，主要有三个配置文件：

`application.conf`：保存应用大部分的配置项，参见：[Configuration file syntax and features](https://www.playframework.com/documentation/2.8.x/ConfigFile)；

`routes`：保存 Play2 的路由规则，即如何将 `uri` 和 `Action` 对应；

`logback.xml`：保存 Logback 日志框架的配置。

### `lib/`

`lib` 是可选的，用于存放需要手动管理的依赖，只需要添加需要的 `JAR` 文件，Play2 会自动把它加入到应用的 `classpath`，参见：[Unmanaged dependencies](https://www.scala-sbt.org/1.x/docs/Library-Dependencies.html)。

### `build.sbt`

类似于 Maven 项目中的 `pom.xml` ，定义项目构建的细节，如：项目名称、版本、依赖等。

### `project/`

保存如下两个文件，用于定义 sbt 的构建规则：

`plugins.sbt`：定义项目使用的 sbt 插件；

`build.properties`：一般用于定义项目使用的 sbt 版本。

### `target/`

保存项目构建产生的文件。

## 开发 `Hello World` 实例程序

参见：[ Implementing Hello World](https://www.playframework.com/documentation/2.8.x/ImplementingHelloWorld)。

使用 Play 创建 `Hello World` 请求很简单，主要有如下几步：

1. 创建 `Hello World` 页面；
2. 创建一个 `Action` 方法；
3. 定义请求映射规则；
4. 测试请求。

### 创建 `Hello World` 页面

在 `app/views` 目录下创建 `hello.scala.html` ，添加如下内容：

```html
@(name: String)
@main("Hello") {
    <section id="top">
        <div class="wrapper">
            <h1>Hello @name</h1>
        </div>
    </section>
}
```

### 创建 `Action` 方法

在 `app/controllers/HomeController` 中添加方法处理请求：

```java
public Result hello(String name) {
    return ok(views.html.hello.render(name));
}
```

### 定义映射规则

在 `conf/routes` 中添加请求 `/hello` 和 `Action` 的映射规则：

```scala
GET        /hello               controllers.HomeController.hello(name:String)
```

### 访问请求

在浏览器输入 `http://localhost:9000/hello?name=MyName` 访问 Hello 页面，如图：

![image-20220421213501584](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/213501.png)

## Play 应用的处理流程

参见：[Play Application Overview](https://www.playframework.com/documentation/2.8.x/PlayApplicationOverview)。

Play 处理 `http://localhost:9000/` 请求的主要步骤如下：

1. 浏览器使用 `GET` 请求访问根路径 `/` ；
2. Play 内部的 HTTP Server 收到请求；
3. Play 使用 `routes` 文件解析请求，将请求映射到对应的 `Action` 方法；
4. `Action` 方法渲染 `index` 页面；
5. HTTP Server 返回 HTML 格式的响应内容。

如下图：

![img](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202204/214438.png)

## Play 示例项目

[Play Tutorials](https://www.playframework.com/documentation/2.8.x/Tutorials) 有很多 Play 的实例项目，供学习使用。

#  Main concepts for Java

参见：[Main concepts for Java](https://www.playframework.com/documentation/2.8.x/JavaHome)。

## 使用 Play `Configuration`

使用依赖注入的方式在项目中使用 Play 的配置对象 `Config`：

```java
package controllers;

import com.typesafe.config.Config;
import play.mvc.Controller;

import javax.inject.Inject;

public class ConfigController extends Controller {
    private final Config config;

    @Inject
    public ConfigController(Config config) {
        this.config = config;
    }
    
    public Result config() {
        return ok(Json.toJson(config));
    }
}
```

## 处理 HTTP 请求

### Play 基础概念：`Action`、`Controller` 和 `Result`

#### `Action`

Play 接收的大部分方法都交由 `Action` 处理，`Action` 用于处理请求并返回结果，如下所示：

```java
public play.mvc.Result index(play.mvc.Http.Request request) {
    return play.mvc.Results.ok("Got request " + request + "!");
}
```

#### `Controller`

Play 中使用的 `Controller` 继承自 `play.mvc.Controllr` ，用于定义一组 `Action` ，如下所示：

```java
package controllers;

import play.*;
import play.mvc.*;

public class Application extends Controller {

  public Result index() {
    return ok("It works!");
  }
  
  public Result hello() {
    return ok("Hello world!");
  }
}
```

#### `Result`

HTTP Response 包括：响应行、响应头和返回数据，Play 中 `play.mvc.Result` 定义了 HTTP 响应结构，`play.mvc.Results` 定义了许多静态方法，方便返回各种 `Result`，如下所示：

```java
// 200 OK
Result ok = ok("Hello world!");
// 404 Not Found
Result notFound = notFound();
Result pageNotFound = notFound("<h1>Page not found</h1>").as("text/html");
// 400 Bad Request
Result badRequest = badRequest(views.html.form.render(formWithErrors));
// 500 Internal Server Error
Result oops = internalServerError("Oops");
// play.mvc.Http.Status
Result anyStatus = status(488, "Strange response type");
// 3XX Redirection
Result redirect = redirect("/user/home");
Result temporaryRedirect =  temporaryRedirect("/user/home");
```

### HTTP 映射

`router` 的作用是将每个请求映射到一个 `Action` 方法的调用，HTTP 请求包括两个部分：

* 请求路径（例如：`/clients/1234`、`/photos/list`），包括查询字符串；
* HTTP 请求方法（`GET`、`POST` 等）。

Play2 的映射定义在 `conf/routes` 文件中，Play2 会编译它，因此可以直接在浏览器中看到错误。映射定义为如下格式：

```properties
Protocol    URLPATH  ControllerMapping
```

最简单的映射如下：

```properties
GET        /                    controllers.HomeController.index()
GET        /hello               controllers.HomeController.hello(name:String)
```

存在动态参数的映射如下：

```properties
# match /bookshop/book/123/
GET /bookshop/book/:id/         controllers.Application.getBook(id:String)

# match 
# /bookshop/book/images/book1.jpg
# /bookshop/book/images/books/book2.jpg
# /bookshop/book/images/books/thumbnails/small/image1.jpg
GET  /bookshop/book/images/*  controllers.Application.fetchImage(name:String)

# $variablename<regular expression>
# match /bookshop/book/10/page/12/
# not match /bookshop/book/10/page/a/
GET  /bookshop/book/:id/page/$page<[0-9]+>/
controllers.Application.fetchBookpage(bookid:String, page:Integer)
```

存在固定参数的映射如下：

```properties
GET /bookshop//authors/    controllers.Application.authors(limit: Integer = 10)
```

存在可选参数的映射如下：

```properties
# variable? = default-value
GET   /bookshop//showcomment/    controllers.Application.showComment(userid ?= null)
```

参见官方文档中有更详细的介绍。

### 处理响应

#### 更改默认的 `Content-Type`

Play2 中 HTTP 响应的 `ContentType` 会根据 Body 中内容的 Java 类型自动推导，例如：

```java
// text/plain
Result textResult = ok("Hello World!");

// application/json
Result jsonResult = ok(Json.toJson(object));
```

可以通过如下方法修改 `Content-Type` ：

```java
public play.mvc.Result as(String contentType) {
}
```

例如：

```java
Result htmlResult = ok("<h1>Hello World!</h1>").as("text/html");
Result htmlResult = ok("<h1>Hello World!</h1>").as(play.mvc.Http.MimeTypes.HTML);
```

#### 更改 HTTP 响应头

通过如下方法修改响应头：

```java
public play.mvc.Result withHeader(String name, String value) {
}
// The headers are processed in pairs, so nameValues(0) is the first header's name, and nameValues(1) is the first header's value, nameValues(2) is second header's name, and so on.
public play.mvc.Result withHeaders(String... nameValues) {
}
```

例如：

```java
public Result index() {
  return ok("<h1>Hello World!</h1>")
      .as(MimeTypes.HTML)
      .withHeader(CACHE_CONTROL, "max-age=3600")
      .withHeader(ETAG, "some-etag-calculated-value");
}
```

#### 使用 `Cookies`

通过如下方法设置 `Cookies` 或删除 `Cookies` ：

```java
// set cookies
public play.mvc.Result withCookies(play.mvc.Http.Cookie... newCookies) {
}
// discarding cookies
public play.mvc.Result discardingCookie(String name) {
}
```

例如：

```java
// add a Cookie
public Result index() {
  return ok("<h1>Hello World!</h1>")
      .as(MimeTypes.HTML)
      .withCookies(
          Cookie.builder("theme", "blue")
              .withMaxAge(Duration.ofSeconds(3600))
              .withPath("/some/path")
              .withDomain(".example.com")
              .withSecure(false)
              .withHttpOnly(true)
              .withSameSite(Cookie.SameSite.STRICT)
              .build());
}

// discard a Cookie
public Result index() {
  return ok("<h1>Hello World!</h1>").as(MimeTypes.HTML).discardingCookie("theme");
}
```

### Session And Flash

保存在 Session 中的数据在整个会话中有效；保存在 Flash 中的数据仅会在下次请求中生效。Play 中 Session 和 Flash 都是通过 Cookies 实现的，所以有几条注意事项：

* 数据的大小不能超过 4K；
* 只能存储字符串内容；
* Cookies 中的内容在浏览器中可见，可能会导致敏感信息泄漏。

#### Session 配置

`play.http.session.cookieName`：Cookie 名称，默认为：`PLAY_SESSION`；

`play.http.session.maxAge`：Session 的超时时间，默认没有超时时间，仅在浏览器关闭时无效。

#### 操作 Session

使用如下方式设置、读取和删除 Session：

```java
public class SessionAndFlashController extends Controller {
    public Result addSession(Http.Request request) {
        // 添加 Session
        return redirect("/session/read").addingToSession(request, "connected", "user@gmail.com");
    }

    public Result readSession(Http.Request request) {
        // 读取 Session
        return request
                .session()
                .get("connected")
                .map(user -> ok("Hello " + user))
                .orElseGet(() -> unauthorized("Oops, you are not connected"));
    }

    public Result clearSession() {
        // 清除 Session
        return redirect("/session/read").withNewSession();
    }
}
```

#### 操作 Flash

Flash 类似 Session，但仅在下一次请求有效。使用如下方式设置、读取 Flash：

```java
public Result addFlash() {
    return redirect("/flash/read").flashing("success", "The item has been created");
}

public Result readFlash(Http.Request request) {
    return ok(request.flash().get("success").orElse("Welcome!"));
}
```

### 请求体解析器

#### 什么是请求体解析器

HTTP 请求的格式是：请求行、请求头和请求体。通常来说请求头很小，能够安全的缓存在内存中，Play 使用 `play.mvc.Http.RequestHeader` 表示请求头。请求体可能很大，因此不能直接缓存在内存中，一般称之为流。事实上很多请求体比较小而且能映射为某种模型缓存在内存中，Play 使用 `play.mvc.BodyParser` 来完成请求体从流到模型的映射。Play 是一个异步框架，因此不能使用传统的 `java.io.InputStream` 来读取请求体，Play 使用 [Akka Stream](https://doc.akka.io/docs/akka/2.6/stream/index.html?language=java&_ga=2.144468661.1268954494.1652018498-216591448.1652018498) 完成这一操作。

#### 使用自带的请求体解析器

没有明确指定时 Play 根据 `Content-Type` 来选择合适的请求体解析器，如下面表格所示：

| Content-Type                                         | Type                                                         | Method                  |
| ---------------------------------------------------- | ------------------------------------------------------------ | ----------------------- |
| `text/plain`                                         | `String`                                                     | `asText()`              |
| `application/json`                                   | `com.fasterxml.jackson.databind.JsonNode`                    | `asJson()`              |
| `application/xml`、`text/xml`、`application/XXX+xml` | `org.w3c.Document`                                           | `asXml()`               |
| `application/x-www-form-urlencoded`                  | `Map<String, String[]>`                                      | `asFormUrlEncoded()`    |
| `multipart/form-data`                                | `play.mvc.Http.MultipartFormData`、`play.mvc.Http.MultipartFormData.FilePart` | `asMultipartFormData()` |
| other                                                | `play.mvc.Http.RawBuffer`                                    | `asRaw()`               |

使用方法如下：

```java
public class BodyParserController extends Controller {
    public Result json(Http.Request request) {
        JsonNode jsonNode = request.body().asJson();
        return ok("Got name: " + jsonNode.get("name").asText());
    }
}
```

#### 明确指定请求体解析器

使用 `@play.mvc.BodyParser.Of` 注解明确指定当前请求的请求体解析器，Play 自带的解析器作为 `play.mvc.BodyParser` 的内部类提供，如下所示：

```java
@play.mvc.BodyParser.Of(BodyParser.Json.class)
public Result json(Http.Request request) {
    JsonNode jsonNode = request.body().asJson();
    return ok("Got name: " + jsonNode.get("name").asText());
}
```

#### 请求体长度限制

`play.http.parser.maxMemoryBuffer`：内存缓存限制，默认100KB；

`play.http.parser.maxDiskBuffer`：磁盘缓存限制，默认10MB。

