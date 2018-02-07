---
title: DispatcherServlet
date: 2017-08-19 00:16:34
categories: 
- spring
- webmvc
- param
tags:
- spring
- spring-webmvc
---


> 本文章介绍`DispatcherServlet`加载过中如何初始化以及配置。

```bash
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath*:application.xml</param-value>
</context-param>

<servlet>
<servlet-name>webapp</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
<servlet-name>webapp</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
```

<!--more-->

Spring MVC web.xml 基本配置

- contextConfigLocation
	> Spring启动时会加载这个内容
- contextClass
	> 加载类，默认使用`org.springframework.web.context.support.XmlWebApplicationContext`
- namespace
	> Spring MVC xml配置文件名，默认${servlet-name}-servlet

	```
	/**
	 * Return the namespace for this servlet, falling back to default scheme if
	 * no custom namespace was set: e.g. "test-servlet" for a servlet named "test".
	 */
	public String getNamespace() {
		return (this.namespace != null ? this.namespace : getServletName() + DEFAULT_NAMESPACE_SUFFIX);
	}
	```
- globalInitializerClasses
	> 初始化ConfigurableApplicationContext定义

	```
	String globalClassNames = getServletContext().getInitParameter(ContextLoader.GLOBAL_INITIALIZER_CLASSES_PARAM);
	if (globalClassNames != null) {
		for (String className : StringUtils.tokenizeToStringArray(globalClassNames, INIT_PARAM_DELIMITERS)) {
			this.contextInitializers.add(loadInitializer(className, wac));
		}
	}

	if (this.contextInitializerClasses != null) {
		for (String className : StringUtils.tokenizeToStringArray(this.contextInitializerClasses, INIT_PARAM_DELIMITERS)) {
			this.contextInitializers.add(loadInitializer(className, wac));
		}
	}

	AnnotationAwareOrderComparator.sort(this.contextInitializers);
	for (ApplicationContextInitializer<ConfigurableApplicationContext> initializer : this.contextInitializers) {
		initializer.initialize(wac);
	}
	```
- publishContext
	```
	if (this.publishContext) {
		// Publish the context as a servlet context attribute.
		String attrName = getServletContextAttributeName();
		getServletContext().setAttribute(attrName, wac);
	}
	```
	> 是否把content放到ServletContext中





