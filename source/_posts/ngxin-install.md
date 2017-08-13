---
title: Nginx 编译安装
date: 2017-08-12 13:39:18
categories:
- nginx
tags:
- nginx
- ubuntu
---

Nginx（发音同engine x）是一个网页服务器，它能反向代理HTTP, HTTPS, SMTP, POP3, IMAP的协议链接，以及一个负载均衡器和一个HTTP缓存。

### 介绍

#### 特点

Nginx是一款面向性能设计的HTTP服务器，相较于Apache、lighttpd具有占有内存少，稳定性高等优势。与旧版本（<=2.2）的Apache不同，nginx不采用每客户机一线程的设计模型，而是充分使用异步逻辑，削减了上下文调度开销，所以并发服务能力更强。整体采用模块化设计，有丰富的模块库和第三方模块库，配置灵活。 在Linux操作系统下，nginx使用epoll事件模型，得益于此，nginx在Linux操作系统下效率相当高。同时Nginx在OpenBSD或FreeBSD操作系统上采用类似于epoll的高效事件模型kqueue。

#### 可大量平行处理

Nginx在官方测试的结果中，能够支持五万个平行连接，而在实际的运作中，可以支持二万至四万个平行链接。

#### nginx 的模块

整体采用模块化设计是nginx的一个重大特点，甚至http服务器核心功能也是一个模块。旧版本的nginx的模块是静态的，添加和删除模块都要对nginx进行重新编译，1.9.11以及更新的版本已经支持动态模块加载。

<!-- more -->

### 安装


#### 安装准备

正式开始前，编译环境gcc g++ 开发库之类的需要提前装好，这里默认你已经装好。

Ubuntu 平台编译环境安装

```
apt-get install build-essential
apt-get install libtool
```

#### 安装依赖包

```bash
apt-get install libpcre3 libpcre3-dev zlib1g-dev libssl-dev build-essential
```


#### 安装源码包

- pcre
- openssl
- zlib

可以安装到任何目录中，这里安装在 `/usr/local/src`

##### pcre 编译 & 安装

- 下载pcre

> wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz

- 编译安装
```bash
$ tar zxvf pcre-8.37.tar.gz
$ cd pcre-8.37/
$ ./configure --prefix=/usr/local
$ make && make install
```

##### openssl 编译 & 安装

- 下载

> wget ftp://ftp.openssl.org/source/openssl-1.0.2h.tar.gz

- 编译安装
```bash
$ tar zxvf openssl-1.0.2h.tar.gz
$ cd openssl-1.0.2h/
$ ./config --prefix=/usr/local --openssldir=/usr/local/ssl
$ make && make install

$ ./config shared --prefix=/usr/local --openssldir=/usr/local/ssl
$ make clean
$ make && make install
```

##### zlib 编译 & 安装

- 下载

> wget http://zlib.net/zlib-1.2.8.tar.gz

- 编译安装

```bash
$ tar zxvf zlib-1.2.8.tar.gz
$ cd zlib-1.2.8/
$ ./configure --prefix=/usr/local
$ make && make install
```

#### 安装 Nginx

Nginx 默认安装在 `/usr/local/nginx`

```bash
$ wget https://nginx.org/download/nginx-1.12.1.tar.gz
$ tar zxvf nginx-1.11.1.tar.gz
$ cd nginx-1.11.1/

$ ./configure --prefix=/usr/local/nginx
--with-http_ssl_module 
--without-http_rewrite_module
--with-stream 
--with-stream_ssl_module
--with-http_gunzip_module
--with-http_stub_status_module 
--with-http_realip
--with-pcre=/usr/local/src/pcre-8.37 
--with-zlib=/usr/local/src/zlib-1.2.8 
--with-openssl=/usr/local/src/openssl-1.0.2h

$ make
$ make install
```

--with-pcre=/usr/local/src/pcre-8.37 指的是 pcre-8.37的源码路径
--with-zlib=/usr/local/src/zlib-1.2.8 指的是 zlib-1.2.8 的源码路径
--with-openssl=/usr/local/src/openssl-1.0.2h指的是 openssl-1.0.2h 的源码路径

### 开机启动 Nginx

```bash
$ cd /etc/init.d/
$ vim nginx
```

添加一下内容到文件中

```
#! /bin/bash
#
# nginx   Start up the nginx server daemon
#
# chkconfig: 2345 55 25
# Description: starts and stops the nginx web server
#
### BEGIN INIT INFO
# Provides:       nginx
# Required-Start:      $all
# Required-Stop:      $all
# Default-Start:        2 3 4 5
# Default-Stop:        0 1 6
# Description:         starts and stops the nginx web server
### END INIT INFO

# To install:
#   copy this file to /etc/init.d/nginx
#   shell> chkconfig –add nginx (RedHat)
#   shell> update-rc.d -f nginx defaults (debian)

# To uninstall:
#   shell> chkconfig –del nginx (RedHat)
#   shell> update-rc.d -f nginx remove

PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/sbin:/usr/local/bin
NAME=nginx
DAEMON=/usr/local/nginx/sbin/$NAME
CONFIGFILE=/usr/local/nginx/conf/$NAME.conf
PIDFILE=/usr/local/nginx/logs/$NAME.pid
ULIMIT=10240

set -e
[ -x “$DAEMON” ] || exit 0

do_start() {
   echo “Starting $NAME …”
   ulimit -SHn $ULIMIT
   $DAEMON -c $CONFIGFILE
}

do_stop() {
   echo “Shutting down $NAME …”
   kill `cat $PIDFILE`
}

do_reload() {
   echo “Reloading $NAME …”
   kill -HUP `cat $PIDFILE`
}

case “$1” in
   start)
     [ ! -f “$PIDFILE” ] && do_start || echo “nginx already running”
  echo -e “.\ndone”
     ;;
   stop)
    [ -f “$PIDFILE” ] && do_stop || echo “nginx not running”
  echo -e “.\ndone”
     ;;
  restart)
  [ -f “$PIDFILE” ] && do_stop || echo “nginx not running”
     do_start
  echo -e “.\ndone”
     ;;
  reload)
   [ -f “$PIDFILE” ] && do_reload || echo “nginx not running”
  echo -e “.\ndone”
     ;;
    *)
    N=/etc/init.d/$NAME
    echo “Usage: $N {start|stop|restart|reload}” >&2
    exit 1
    ;;
esac

exit 0
```

添加执行权限

```bash
$ chmod a+x nginx
```

添加启动项

```bash
$ update-rc.d -f nginx remove (Debian)
$ chkconfig --del nginx (RedHat)
```

卸载启动项(仅卸载使用)

```bash
$ update-rc.d -f nginx remove (Debian)
$ chkconfig --del nginx (RedHat)
```
