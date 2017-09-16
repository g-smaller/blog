---
title: spring-aop-aspectJ
date: 2017-08-23 00:12:03
categories:
- spring
- AOP
- aspectJ
tags:
- Spring
- AOP
- aspectJ
---


AOP 面向切面编程，相对于OOP面向对象编程。

Spring AOP 的存在目的是为了解耦。AOP可以让一组类共享相同的行为。在OOP中只能通过继承类和实现接口，来使代码的耦合度增强，且类继承只能为单继承，阻碍更多行为添加到一组类上，AOP弥补了OOP的不足。

Spring 支持AspectJ的注解式切面编程。

- 使用`@Aspect`声明式一个切面
- 使用`@Before` `@After` `@Around`定义建言(advice)，可直接将拦截规则(切点)作为参数。
- 其中`@Before` `@After` `@Around`参数的拦截规则为切点(PointCut)，为了使切点复用，可使用`@PointCut`专门定义拦截规则，然后在`@Before` `@After` `@Around`参数中调用
- 其中符合条件的每一个拦截处为连接点(JoinPoint)
<!--more-->

[使用Spring进行面向切面编程（AOP）](http://man.lupaworld.com/content/develop/spring_ref/2.0/html/aop.html)

### 切点
#### execution()
> 格式`execution(<修饰符模式>? <返回类型模式> <方法名模式>(<参数模式>) <异常模式>?)`
> 除了返回类型模式、方法名模式和参数模式外，其它项都是可选的

例子：execution(* com.keng.web.test.spring..*.*Service.*(..))

- execution()
	> 表达式主体
- 第一个`*`号
	> 表示返回类型，`*`号表示所有的类型
- 包名
	> 表示需要拦截的包名，后面的两个点`..`表示当前包和当前包的所有子包；
- 第二个`*`号		
	> 表示类名，`*`号表示所有类；上面的表达式表示已`Service`结尾的类
- *(..)
	> 最后的表达式星号`*`表示方法名，括号里面表示方法的参数，连个点`..`表示任何参数

#### 定义切点详解
- `execution(public * *(..))`
	> 匹配所有目标类的public方法
- `execution(* *Dao(..))`
	> 匹配目标所有以Dao为后缀的方法
- ` execution(* com..*.*Dao.find*(..))`
	> 匹配com包下以及子包下以`Dao`为结尾，以`find`为前缀的方法；
- `execution(* com.Animal.*(..))`
	> 匹配`Animal`接口以及所有实现类的方法；
- `execution(* com.Animal+.*(..))`
	> 不仅匹配`Animal`接口以及所有实现类的方法；还匹配实现类自定义的方法。
- `execution(* invoke(String,int)))`
	> 匹配joke(String,int)方法，且joke()方法的第一个入参是String，第二个入参是int;
	> 如果方法中的入参类型是java.lang包下的类，可以直接使用类名，否则必须使用全限定类名，如joke(java.util.List,int)；
- `execution(* invoke(String,..)))`
	> 匹配目标类中的invoke()方法，该方法第 一个入参为String，后面可以有任意个入参且入参类型不限;
- `execution(* invoke(Object+)))`
	> 匹配目标类中的invoke()方法，方法拥有一个入参，且入参是Object类型或该类的子类。它匹配invoke(String s1)和invoke(Client c)。如果我们定义的切点是execution(* joke(Object))，则只匹配invoke(Object object)而不匹配invoke(String cc)或invoke(Client c)。

### 添加依赖

```
<!-- Spring AOP 支持 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>4.1.5.RELEASE
</dependency>
<!-- AspectJ 支持 -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjrt</artifactId>
    <version>1.8.5</version>
</dependency>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.5</version>
</dependency>
```

### 拦截器

#### 通过注解的方式

##### 拦截规则的注解

```
import java.lang.annotation.*;

/**
 * Created by guoguo on 2017/8/22.
 */
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Action {

    String name() default "";

}
```

注解本身是没有功能的，就和xml一样。注解和xml都是一种元数据。元数据即解释数据的数据，这就是所谓的配置。

注解的功能来自用这个注解的地方。

##### 编写使用注解的拦截类

```
import org.springframework.stereotype.Service;

/**
 * Created by guoguo on 2017/8/22.
 */
@Service
public class DemoAnnotionService {

    @Action(name = "注解式拦截的 Add 操作")
    public void add(){}

}
```

##### 编写切面

```
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;


@Aspect
@Component
public class LogAspect {

    private Logger LOGGER = LoggerFactory.getLogger(getClass());

    /**
      * 定义切点 - 拦截规则
      * 
      */
    @Pointcut("@annotation(com.keng.web.test.spring.aop.Action)")
    public void annotationPointCut(){}

    /*
     * 定义建言
     * 调用切点
     * 
     * @param joinPoint 连接点
     */
    @After("annotationPointCut()")
    public void after(JoinPoint joinPoint) {
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();

        Method method = signature.getMethod();

        Action annotation = method.getAnnotation(Action.class);
        LOGGER.info("注解式拦截： " + annotation.name());
    }
}
```

#### 基于方法规则

##### 编写使用方法规则被拦截类

```
import org.springframework.stereotype.Service;

/**
 * Created by guoguo on 2017/8/22.
 */
@Service
public class DemoAnnotionService {

    public void add(){}

}
```
##### 编写切面

```
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.reflect.MethodSignature;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;

@Aspect
@Component
public class LogAspect {

    private Logger LOGGER = LoggerFactory.getLogger(getClass());

    @Before("execution(* com.keng.web.test.spring..*.*Service.*(..))")
    public void before(JoinPoint joinPoint) {
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();

        Method method = signature.getMethod();
        LOGGER.info("方法规则式拦截： " + method.getName());
    }


}

```


### 拦截类注解说明

- `@Aspect`
	> 声明一个切面

- `@Component`
	> 让此切面成为Spring容器管理的Bean

- `@PointCut`
	> 声明切点

- `@After`
	> 声明一个建言，并使用@PointCut定义的切点；
	> 该建言在拦截之后执行

- `@Before`	
	> 声明一个建言，直接使用拦截规则作为参数；
	> 该建言在拦截之前执行

### 配置拦截支持

```
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@EnableAspectJAutoProxy
@ComponentScan("com.keng.web.test.spring")
public class Configable {

}
```

- `@Configuration`
	> 当前类是一个配置类，该类里面可以存在0个或者多个@Bean注解
	> 可以理解为Spring的XML

- `@EnableAspectJAutoProxy`
	> 开启Spring对AspectJ的支持
- `@ComponentScan`
	> 告诉Spring 哪个packages 的用注解标识的类 会被spring自动扫描并且装入bean容器。
	> 如果不加上@ComponentScan，自动扫描该controller，那么该Controller就不会被spring扫描到，更不会装入spring容器中，因此你配置的这个Controller也没有意义。

### 测试

```
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Configable.class);

DemoAnnotionService bean = context.getBean(DemoAnnotionService.class);
bean.add();

DemoMethodService bean1 = context.getBean(DemoMethodService.class);
bean1.add();

context.close();
```