---
title: web-jetty-maven-plugin
date: 2017-08-20 23:19:00
categories:
- web
- jetty
- maven
- plugin
tags:
- jetty
- maven
- plugin
---

我们创建Java Web项目时，都需要下载Web容器(Tomcat)来启动项目；但是在本地开发过程中，我们可不可简化一下步骤，不下载Web容器和配置启动项，而在项目中使用命令就可以实现。

### Jetty

> 一个Web容器

### Maven

> 项目管理工具


Jetty配合Maven可以简化Web项目的配置；前提是你已经学习了解了Maven。

<!--more-->

### 配置
```bash
<plugin>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-maven-plugin</artifactId>
    <version>${jetty.version}</version>
    <configuration>
        <httpConnector>
            <port>8080</port>
            <idleTimeout>60000</idleTimeout>
        </httpConnector>
        <webApp>
            <contextPath>/</contextPath>
            <!-- 加载web.xml
            <descriptor>${project.basedir}/src/main/webapp/web.xml</descriptor>
             -->
            <!--<defaultsDescriptor></defaultsDescriptor>-->
        </webApp>
        <stopKey>shutdown</stopKey>
        <stopPort>9999</stopPort>
        <!--
            默认值是 0。大于 0 的数值表示开启，0 表示关闭，单位为秒。以配置数值为一个周期，自动的扫描文件检查其内容是否有变化，如果发现文件的
            内容被改变，则自动重新部署运用
        -->
        <scanIntervalSeconds>30</scanIntervalSeconds>
        <!--
            默认值为 automatic，它与大于 0 的 scanIntervalSeconds 节点一起作用，实现自动热部署的工作。设为 manual 的好处是，当你改变文件
            内容并保存时，不会马上触发自动扫描和重部署的动作，你还可以继续的修改，直至你在 Console 或命令行中敲回车键（Enter）的时候才触发重
            新加载的动作。这样可以更加的方便调试修改
        -->
        <reload>manual</reload>
        <!--
            dumpOnStart 默认值为 false，如果设为 true，jetty 在启动时会把当前服务进程的内存信息输出到控制台中，但这并不会保存到文件中
        -->
        <dumpOnStart>true</dumpOnStart>
        <requestLog implementation="org.eclipse.jetty.server.NCSARequestLog">
            <filename>target/access-jetty-yyyy_MM_dd.log</filename>
            <filenameDateFormat>yyyy_MM_dd</filenameDateFormat>
            <logDateFormat>yyyy-MM-dd HH:mm:ss</logDateFormat>
            <logTimeZone>GMT+8:00</logTimeZone>
            <append>true</append>
            <logServer>true</logServer>
            <retainDays>120</retainDays>
            <logCookies>true</logCookies>
        </requestLog>
    </configuration>
</plugin>
```

> `${jetty.version}`是Jetty版本

- 可以使用`9.3.12.v20160915`替换`${jetty.version}`;
- 使用配置项
	```
	<properties>
		<jetty.version>9.3.12.v20160915</jetty.version>
	</properties>
	```

### 启动项目

> mvn jetty:run