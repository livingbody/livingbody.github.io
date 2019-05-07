---
layout: post
title:  "SpringBoot学习"
categories: java
tags: SpringBoot
author: livingbody
---







# SpringBoot学习

## springboot运行必须在JDK8以上环境

## springboot parent 整合第三方常用框架依赖信息

## 使用springboot 2.1.3 Realse

## springcloud依赖于springboot实现微服务，springboot默认集成springmvc

## 微服务http接口 http+json格式

## @RestController表示该类的所有方法返回json格式

http://localhost:8080/memberIndex

## @SpringBootApplication等同于扫同级包和子包

## SpringBoot访问静态资源

http://localhost:8080/1.png对应于static/1.png

## SpringBoot 整合JSP， 一定要使用war包

不要把jsp页面存放在resource下。

注意目录：src/main/webapp/WEB-INF

```properties
#添加jsp支持
spring.mvc.view.prefix=/WEB-INF/jsp
spring.mvc.view.suffix=.jsp
```

```xml
    <!--用于编译jsp-->
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-jasper</artifactId>
    </dependency>
</dependencies>
```

```java
package cn.goingtodo.hello_world_springboot;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.HashMap;
import java.util.Map;

@ControllerAdvice(basePackages = "cn.goingtodo.hello_world_springboot")
public class GlobalExceptionController {
    //    返回json，modeAndView返回页面
    @ExceptionHandler(RuntimeException.class)
    @ResponseBody
    public Map<String, Object> errorResult() {
        Map<String, Object> errorResultMap = new HashMap<String, Object>();
        errorResultMap.put("errorCode", "500");
        errorResultMap.put("errorMsg", "系统错误！");
        return errorResultMap;
    }
}
```

## log4j 日志记录

```java
package cn.goingtodo.hello_world_springboot;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.Enumeration;

@Aspect
@Component
public class WebLogAspect {
    private static final Logger logger = LoggerFactory.getLogger(WebLogAspect.class);

    @Pointcut("execution (public * cn.goingtodo.hello_world_springboot.*.*(..))")
    public void webLog() {

    }

    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint) throws Throwable {
        // 接收到请求，记录请求内容，多半存储到nosql数据库
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        // 记录下请求内容
        logger.info("URL : " + request.getRequestURL().toString());
        logger.info("HTTP_METHOD : " + request.getMethod());
        logger.info("IP : " + request.getRemoteAddr());
        Enumeration<String> enu = request.getParameterNames();
        while (enu.hasMoreElements()) {
            String name = (String) enu.nextElement();
            logger.info("name:{},value:{}", name, request.getParameter(name));
        }
        //传统写磁盘有很大缺点，分布式情况服务器集群，20台服务器，日志存在各个server上，无法顺畅查看
        //分布式日志收集
    }

        @AfterReturning(returning = "ret", pointcut = "webLog()")
        public void doAfterReturning (Object ret) throws Throwable {
            // 处理完请求，返回内容
            logger.info("RESPONSE : " + ret);
        }
    }
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

```properties
### 设置###
log4j.rootLogger=debug,stdout,D,E,I
### 输出信息到控制抬 ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=[%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n
### 输出DEBUG 级别以上的日志到=E://logs/error.log ###
log4j.appender.D=org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File=./log.log
log4j.appender.D.Append=true
log4j.appender.D.Threshold=DEBUG
log4j.appender.D.layout=org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
### 输出ERROR 级别以上的日志到=E://logs/error.log ###
log4j.appender.E=org.apache.log4j.DailyRollingFileAppender
log4j.appender.E.File=./error.log
log4j.appender.E.Append=true
log4j.appender.E.Threshold=ERROR
log4j.appender.E.layout=org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
### 输出INFO 级别以上的日志到=E://logs/error.log ###
log4j.appender.I=org.apache.log4j.DailyRollingFileAppender
log4j.appender.I.File=./info.log
log4j.appender.I.Append=true
log4j.appender.I.Threshold=INFO
log4j.appender.I.layout=org.apache.log4j.PatternLayout
log4j.appender.I.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```

## lombok插件

```java
package cn.goingtodo.hello_world_springboot.entity;

import lombok.Data;
import lombok.extern.slf4j.Slf4j;

@Slf4j//添加日志
@Data
public class User {
    private String name;
    private Integer age;

    //lombok 底层使用字节码技术ASM修改字节码文件，生成get和set方法

    public  static void main(String[] args){
        User user=new User();
        user.setAge(36);
        user.setName("livingbody");
        log.info(user.toString());
    }

}
```

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

最终编译时，还是生成get/set方法。生产环境就不用lombok插件了，因为已经编译了。

## 异步调用

```java
//SpringBootApplication等同于扫同级包和子包
@EnableAsync//开启异步调用
@SpringBootApplication
public class HelloWorldSpringbootApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloWorldSpringbootApplication.class, args);
    }

}
```

```java
package cn.goingtodo.hello_world_springboot.service;

import lombok.extern.slf4j.Slf4j;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
@Slf4j
public class MemberService {

    //    添加用户时发送邮件
    @Async //相当于这个方法是单独的线程在执行
    public String addMemberAndEmail() {
        log.info("******2...send user email");
        try{
            Thread.sleep(5000);
        }catch (Exception e){

        }
        log.info("******3 ...line");
        return "send Email Success";
    }
}
```

## 自定义参数

```properties
#添加jsp支持
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

name= livingbody
```

```java
package cn.goingtodo.hello_world_springboot;

import cn.goingtodo.hello_world_springboot.service.MemberService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

//演示异步调用
@Slf4j
//@RestController表示该类的所有方法返回json格式
@RestController
public class MemberController {

    @Autowired
    private MemberService memberService;

    @Value("${name}")
    private String name;

    @RequestMapping("/getName")
    public String getName() {
        return this.name;
    }

    @RequestMapping("/memberIndex")
    public String memberIndex() {
        return "Hello World,springboot2.1.3全新教程";
    }

    @RequestMapping("/addMemberAndEmail")
    public String addMemberAndEmail() {
        log.info("1...line.");
        String result = memberService.addMemberAndEmail();
        log.info("***...4...line..");
        return result;
    }

}
```

## 环境设置

1.本地开发环境dev

2.测试环境 test

3.预生产环境 pre

4.生产环境prd

5.分布式配置中心---动态获取配置文件中心

```properties
spring.profiles.active=dev
application.properties
application-dev.properties
application-test.properties
application-prd.properties
...
```



