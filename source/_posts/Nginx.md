---
title: Nginx
date: 2020-12-03 20:39:51
categories: learn
tags: nginx
---

# Nginx

## Nginx 使用场景

### HTTP 的反向代理服务器

* 正向代理

  ![image-20201203210919188](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202012/210919.png)

* 反向代理

  ![image-20201203211044613](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202012/211044.png)

* Nginx 的反向代理

  ![image-20201203211409241](https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202012/211409.png)

### 动态静态资源分离

* 静态资源无需经过 Tomcat，Tomcat 只负责处理动态请求
* Nginx 本身是一个静态资源服务器

## Nginx 的优点

* 高并发、高性能
* 可扩展性好：模块化设计
* 可靠性高
* 热部署：不停止服务的情况下升级 Nginx
* 开源、可商用

## Nginx 安装使用

### Nginx 在 CentOS7 下的安装

* 安装 yum-utils

  `yum install yum-utils`

* 为 Nginx 增加源

  ```shell
  [root@localhost ~]# vi /etc/yum.repos.d/nginx.repo
  
  [nginx-stable]
  name=nginx stable repo
  baseurl=http://nginx.org/packages/centos/7/$basearch/
  gpgcheck=1
  enabled=1
  gpgkey=https://nginx.org/keys/nginx_signing.key
  module_hotfixes=true
  
  [nginx-mainline]
  name=nginx mainline repo
  baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
  gpgcheck=1
  enabled=0
  gpgkey=https://nginx.org/keys/nginx_signing.key
  module_hotfixes=true
  ```

* 查看 Nginx 源

  ```shell
  [root@localhost ~]# yum list | grep nginx
  nginx.x86_64                                1:1.18.0-2.el7.ngx         @nginx-stable
  nginx-debug.x86_64                          1:1.8.0-1.el7.ngx          nginx-stable
  nginx-debuginfo.x86_64                      1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-geoip.x86_64                   1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-geoip-debuginfo.x86_64         1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-image-filter.x86_64            1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-image-filter-debuginfo.x86_64  1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-njs.x86_64                     1:1.18.0+0.5.0-1.el7.ngx   nginx-stable
  nginx-module-njs-debuginfo.x86_64           1:1.18.0+0.5.0-1.el7.ngx   nginx-stable
  nginx-module-perl.x86_64                    1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-perl-debuginfo.x86_64          1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-xslt.x86_64                    1:1.18.0-2.el7.ngx         nginx-stable
  nginx-module-xslt-debuginfo.x86_64          1:1.18.0-2.el7.ngx         nginx-stable
  nginx-nr-agent.noarch                       2.0.0-12.el7.ngx           nginx-stable
  pcp-pmda-nginx.x86_64                       4.3.2-12.el7               base
  ```

* 安装指定版本

  ```shell
  [root@localhost ~]# yum install nginx 1:1.18.0-2.el7.ngx
  ```

* 验证 Nginx 是否安装成功

  ```shell
  [root@localhost ~]# nginx -v
  nginx version: nginx/1.18.0
  ```

* 可能需要配置防火墙

  ```shell
  # 查看防火墙状态
  [root@localhost conf.d]# firewall-cmd --state
  running
  # 查看启用的端口
  [root@localhost conf.d]# firewall-cmd --list-port
  80/tcp
  # 开放 80 端口 --permanent 永久生效
  [root@localhost conf.d]# firewall-cmd --add-port=80/tcp --permanent
  ```

* 或者同时打开 80 和443端口

  ```shell
  firewall-cmd --permanent --zone=public --add-service=http
  firewall-cmd --permanent --zone=public --add-service=https
  firewall-cmd --reload
  ```

### Nginx 常用命令

```shell
[root@localhost ~]# nginx -h
nginx version: nginx/1.18.0
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /etc/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```

| 命令     | 含义                     |
| -------- | ------------------------ |
| `?/h`    | 显示帮助                 |
| `v/V`    | 显示版本信息             |
| `c`      | 使用指定的配置文件       |
| `g`      | 指定配置指令             |
| `p`      | 指定运行目录             |
| `t/T`    | 测试配置文件编写是否正确 |
| `s`      | 向 master 进程发送命令   |
| `stop`   | 立即停止                 |
| `quit`   | 优雅停止                 |
| `reopen` | 重新开始记录日志文件     |
| `reload` | 重启                     |

### Nginx 配置文件

* Nginx 相关的配置文件在`/etc/nginx/`目录中 
* Nginx 的主配置文件是`/etc/nginx/nginx.conf`
* Nginx 日志文件`access.log`和`error.log`位于`/var/log/nginx/`目录下

* 基础语法：

  * 使用`;`结尾
  * 使用`{}`组织多条指令
  * 使用`include`引入其它配置文件
  * 使用`#`注释
  * `$`表示变量

* Nginx 默认配置文件及注释`nginx.conf`

  ```nginx
  user  nginx;
  worker_processes  1;
  
  error_log  /var/log/nginx/error.log warn;
  pid        /var/run/nginx.pid;
  
  
  events {
      # 最大连接数
      worker_connections  1024;
  }
  
  
  http {
      include       /etc/nginx/mime.types;
      default_type  application/octet-stream;
      # 日志格式
      log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';
  
      # 访问日志
      access_log  /var/log/nginx/access.log  main;
  
  
      sendfile        on;
      tcp_nopush     on;
  
      keepalive_timeout  65;
  
      #gzip  on;
  
      include /etc/nginx/conf.d/*.conf;
  }
  ```

* `default.conf`

  ```nginx
  server {
      listen       80;
      # server_name  localhost;
  
      #charset koi8-r;
      #access_log  /var/log/nginx/host.access.log  main;
  
      location / {
          root   /usr/share/nginx/html;
          index  index.html index.htm;
      }
  
      #error_page  404              /404.html;
  
      # redirect server error pages to the static page /50x.html
      #
      error_page   500 502 503 504  /50x.html;
      location = /50x.html {
          root   /usr/share/nginx/html;
      }
  
      # proxy the PHP scripts to Apache listening on 127.0.0.1:80
      #
      #location ~ \.php$ {
      #    proxy_pass   http://127.0.0.1;
      #}
  
      # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
      #
      #location ~ \.php$ {
      #    root           html;
      #    fastcgi_pass   127.0.0.1:9000;
      #    fastcgi_index  index.php;
      #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
      #    include        fastcgi_params;
      #}
  
      # deny access to .htaccess files, if Apache's document root
      # concurs with nginx's one
      #
      #location ~ /\.ht {
      #    deny  all;
      #}
  }
  ```

  
