---
title: web-jstl-problem
date: 2017-08-25 23:31:20
categories:
- web
- jstl
tags:
- jstl
---

今天在使用Maven配置JSP、JSTL时，出现了JSTL不能解析的问题，上网查找了一些资料，于是把问题整理出来；

使用IDEA集成Maven创建Web项目时，web.xml默认的版本是2.3，而我添加的依赖是Servlet依赖是3.1.0的，在JSP使用EL表达式是，不能解析；

### 依赖
```
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>jstl</artifactId>
  <version>1.2</version>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>3.1.0</version>
  <scope>provided</scope>
</dependency>
<dependency>
  <groupId>javax.servlet.jsp</groupId>
  <artifactId>jsp-api</artifactId>
  <version>2.2</version>
  <scope>provided</scope>
</dependency>
```

<!--more-->

各版本的区别：JSTL1.0(不支持EL)、JSTL 1.1和JSTL 1.2支持的servlet和jsp规范也不同：web.xml中要申明相应的servlet版本：

### JSTL版本

#### 1.0
> JSTL1.0和JSP1.2 需要Servlet2.3

```
<?xml version=”1.0″ encoding=”UTF-8″?>
<!DOCTYPE web-app
PUBLIC “-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN”
“http://Java.sun.com/dtd/web-app_2_3.dtd”>

<web-app>

</web-app>
```

使用方法：
> <%@ taglib uri="http://java.sun.com/jstl/core" prefix="c" %>

### 1.1 
> JSTL1.1 和JSP 2.0 需要Servlet2.4

```
<?xml version=”1.0″ encoding=”UTF-8″?>
<web-app 
	xmlns=”http://java.sun.com/xml/ns/j2ee”
	xmlns:xsi=”http://www.w3.org/2001、XMLSchema-instance”
	xmlns:web=”http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd”
	xsi:schemaLocation=”http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd ”
	version=”2.4″>
</web-app>
```

使用方法：
> <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

### 1.2

> JSTL1.2需要servlet2.5 servlet3.0 servlet3.1

#### Servlet 2.5

```
<?xml version=”1.0″ encoding=”UTF-8″?>
<web-app 
	xmlns=”http://java.sun.com/xml/ns/javaee”
	xmlns:xsi=”http://www.w3.org/2001/XMLSchema-instance”
	xmlns:web=”http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd”
	xsi:schemaLocation=”http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd”
	id=”WebApp_ID” version=”2.5″>

</web-app>
```

#### Servlet 3.0

```
<?xml version=”1.0″ encoding=”utf-8″?>
<web-app 
	version=”3.0″
	xmlns=”http://java.sun.com/xml/ns/javaee”
	xmlns:xsi=”http://www.w3.org/2001/XMLSchema-instance”
	xsi:schemaLocation=”http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd”>

</web-app>
```

#### Servlet 3.1

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
     version="3.1">
```

使用方法：
> <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

[web.xml 版本说明](http://www.mkyong.com/web-development/the-web-xml-deployment-descriptor-examples/)

### EL表达式的支持：

> 默认开启支持EL表达式支持有：

- servlet2.4

- J2EE6

### 默认不支持EL表达式需要进行如下2种配置中的其一来开启EL：

- 在JSP中加入：

> <%@ page isELIgnored="false"%>

- 在web.xm中加入：

```
<jsp-config>     
    <jsp-property-group>     
        <url-pattern>*.jsp</url-pattern>     
        <el-ignored>false</el-ignored>     
    </jsp-property-group>     
</jsp-config>
```
