---
title: Java Maven Nexus 环境搭建
date: 2017-08-12 14:15:19
categories:
- jdk
- maven
- nexus
tags:
- jdk
- maven
- nexus
---


### 下载&安装

- JDK
- Maven
- Nexus

#### JDK

下载

```bash
$ wget http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jdk-8u144-linux-x64.tar.gz
```

安装

```bash
$ tar -zxvf jdk-8u144-linux-x64.tar.gz -C /usr/local
$ ln -s /usr/local/jdk1.8.0_144 /usr/local/jdk
```

<!--more-->

#### Maven

下载

```bash
$ wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.tar.gz
```

安装

```bash
$ tar -zxvf apache-maven-3.5.0-bin.tar.gz -C /usr/local
$ ln -s /usr/local/apache-maven-3.5.0 /usr/local/maven
```

#### Nexus

> Nexus 有两个版本2.x和3.x，你可以按照自己的意愿来下载安装任意一个版本

[下载](https://www.sonatype.com/download-oss-sonatype)

2.x

```bash
$ wget https://sonatype-download.global.ssl.fastly.net/nexus/oss/nexus-2.14.5-02-bundle.tar.gz
```


3.0

```bash
$ wget https://sonatype-download.global.ssl.fastly.net/nexus/3/nexus-3.5.0-02-unix.tar.gz
```

### 设置环境变量

#### JDK

```bash
$ cd /etc/profile.d
$ touch jdk.sh
$ vim jdk.sh
# 文件内容
export JAVA_HOME=/usr/local/jdk
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
```

#### Maven
```
$ cd /etc/profile.d
$ touch ven.sh
$ vim maven.sh
# 文件内容
export M2_HOME=/usr/local/maven
export PATH=$M2_HOME/bin:$PATH
export MAVEN_OPTS="-Xms256m -Xmx512m"
```






