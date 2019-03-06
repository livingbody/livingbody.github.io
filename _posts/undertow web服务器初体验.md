---
layout: post
title:  "undertow web服务器初体验"
date:   2018-03-06 10:00:00
categories: undertow web
tags: undertow web
author: livingbody
---


#  undertow web服务器初体验

## 1.Undertow介绍

Undertow is a web server designed to be used for both blocking and non-blocking tasks. Some of its main features are:

- High Performance
- Embeddable
- Servlet 4.0
- Web Sockets
- Reverse Proxy

There and two main ways that Undertow can be used, either by directly embedding it in your code, or as part of the [Wildfly Application Server](http://wildfly.org). This guide mostly focuses on the embedded API’s, although a lot of the content is still relevant if you are using Wildfly, it is just that the relevant functionality will generally be exposed via XML configuration rather than programatic configuration.

The documentation is broken up into two parts, the first part focuses on Undertow code, while the second focuses on Servlet.
## 2.springboot整合
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-undertow</artifactId>
        </dependency>
​```xml


```

## 3.springboot 在undertow下的运行效率

可见一个2.372s，一个2.751s，的确undertow快了一点点，再加上它的高并发，略胜tomcat。

```bash
"C:\Program Files\Java\jdk-11.0.2\bin\java.exe" -XX:TieredStopAtLevel=1 -noverify -Dspring.output.ansi.enabled=always -Dcom.sun.management.jmxremote -Dspring.liveBeansView.mbeanDomain -Dspring.application.admin.enabled=true "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2018.3.4\lib\idea_rt.jar=11561:C:\Program Files\JetBrains\IntelliJ IDEA 2018.3.4\bin" -Dfile.encoding=UTF-8 -classpath C:\Users\pc\IdeaProjects\springboot03\target\classes;D:\maven\repository\org\springframework\boot\spring-boot-starter\2.1.3.RELEASE\spring-boot-starter-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot\2.1.3.RELEASE\spring-boot-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\spring-context\5.1.5.RELEASE\spring-context-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-autoconfigure\2.1.3.RELEASE\spring-boot-autoconfigure-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-logging\2.1.3.RELEASE\spring-boot-starter-logging-2.1.3.RELEASE.jar;D:\maven\repository\ch\qos\logback\logback-classic\1.2.3\logback-classic-1.2.3.jar;D:\maven\repository\ch\qos\logback\logback-core\1.2.3\logback-core-1.2.3.jar;D:\maven\repository\org\apache\logging\log4j\log4j-to-slf4j\2.11.2\log4j-to-slf4j-2.11.2.jar;D:\maven\repository\org\apache\logging\log4j\log4j-api\2.11.2\log4j-api-2.11.2.jar;D:\maven\repository\org\slf4j\jul-to-slf4j\1.7.25\jul-to-slf4j-1.7.25.jar;D:\maven\repository\javax\annotation\javax.annotation-api\1.3.2\javax.annotation-api-1.3.2.jar;D:\maven\repository\org\springframework\spring-core\5.1.5.RELEASE\spring-core-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-jcl\5.1.5.RELEASE\spring-jcl-5.1.5.RELEASE.jar;D:\maven\repository\org\yaml\snakeyaml\1.23\snakeyaml-1.23.jar;D:\maven\repository\org\springframework\boot\spring-boot-devtools\2.1.3.RELEASE\spring-boot-devtools-2.1.3.RELEASE.jar;D:\maven\repository\org\projectlombok\lombok\1.18.6\lombok-1.18.6.jar;D:\maven\repository\org\slf4j\slf4j-api\1.7.25\slf4j-api-1.7.25.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-web\2.1.3.RELEASE\spring-boot-starter-web-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-json\2.1.3.RELEASE\spring-boot-starter-json-2.1.3.RELEASE.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-databind\2.9.8\jackson-databind-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-annotations\2.9.0\jackson-annotations-2.9.0.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-core\2.9.8\jackson-core-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\datatype\jackson-datatype-jdk8\2.9.8\jackson-datatype-jdk8-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\datatype\jackson-datatype-jsr310\2.9.8\jackson-datatype-jsr310-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\module\jackson-module-parameter-names\2.9.8\jackson-module-parameter-names-2.9.8.jar;D:\maven\repository\org\hibernate\validator\hibernate-validator\6.0.14.Final\hibernate-validator-6.0.14.Final.jar;D:\maven\repository\javax\validation\validation-api\2.0.1.Final\validation-api-2.0.1.Final.jar;D:\maven\repository\org\jboss\logging\jboss-logging\3.3.2.Final\jboss-logging-3.3.2.Final.jar;D:\maven\repository\com\fasterxml\classmate\1.4.0\classmate-1.4.0.jar;D:\maven\repository\org\springframework\spring-web\5.1.5.RELEASE\spring-web-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-beans\5.1.5.RELEASE\spring-beans-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-webmvc\5.1.5.RELEASE\spring-webmvc-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-aop\5.1.5.RELEASE\spring-aop-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-expression\5.1.5.RELEASE\spring-expression-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-undertow\2.1.3.RELEASE\spring-boot-starter-undertow-2.1.3.RELEASE.jar;D:\maven\repository\io\undertow\undertow-core\2.0.17.Final\undertow-core-2.0.17.Final.jar;D:\maven\repository\org\jboss\xnio\xnio-api\3.3.8.Final\xnio-api-3.3.8.Final.jar;D:\maven\repository\org\jboss\xnio\xnio-nio\3.3.8.Final\xnio-nio-3.3.8.Final.jar;D:\maven\repository\io\undertow\undertow-servlet\2.0.17.Final\undertow-servlet-2.0.17.Final.jar;D:\maven\repository\org\jboss\spec\javax\annotation\jboss-annotations-api_1.2_spec\1.0.2.Final\jboss-annotations-api_1.2_spec-1.0.2.Final.jar;D:\maven\repository\io\undertow\undertow-websockets-jsr\2.0.17.Final\undertow-websockets-jsr-2.0.17.Final.jar;D:\maven\repository\org\jboss\spec\javax\websocket\jboss-websocket-api_1.1_spec\1.1.3.Final\jboss-websocket-api_1.1_spec-1.1.3.Final.jar;D:\maven\repository\javax\servlet\javax.servlet-api\4.0.1\javax.servlet-api-4.0.1.jar;D:\maven\repository\org\glassfish\javax.el\3.0.0\javax.el-3.0.0.jar cn.goingtodo.main.MainApplication

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)

2019-03-04 23:37:46.008  INFO 3120 --- [  restartedMain] cn.goingtodo.main.MainApplication        : Starting MainApplication on livingbody with PID 3120 (C:\Users\pc\IdeaProjects\springboot03\target\classes started by pc in C:\Users\pc\IdeaProjects\springboot03)
2019-03-04 23:37:46.008  INFO 3120 --- [  restartedMain] cn.goingtodo.main.MainApplication        : No active profile set, falling back to default profiles: default
2019-03-04 23:37:46.055  INFO 3120 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2019-03-04 23:37:46.055  INFO 3120 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2019-03-04 23:37:47.332  WARN 3120 --- [  restartedMain] io.undertow.websockets.jsr               : UT026010: Buffer pool was not set on WebSocketDeploymentInfo, the default pool will be used
2019-03-04 23:37:47.364  INFO 3120 --- [  restartedMain] io.undertow.servlet                      : Initializing Spring embedded WebApplicationContext
2019-03-04 23:37:47.364  INFO 3120 --- [  restartedMain] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1294 ms
2019-03-04 23:37:47.614  INFO 3120 --- [  restartedMain] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-03-04 23:37:47.786  INFO 3120 --- [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2019-03-04 23:37:47.817  INFO 3120 --- [  restartedMain] org.xnio                                 : XNIO version 3.3.8.Final
2019-03-04 23:37:47.832  INFO 3120 --- [  restartedMain] org.xnio.nio                             : XNIO NIO Implementation Version 3.3.8.Final
2019-03-04 23:37:47.895  INFO 3120 --- [  restartedMain] o.s.b.w.e.u.UndertowServletWebServer     : Undertow started on port(s) 8080 (http) with context path ''
2019-03-04 23:37:47.911  INFO 3120 --- [  restartedMain] cn.goingtodo.main.MainApplication        : Started MainApplication in 2.372 seconds (JVM running for 3.504)
```

## 4.springboot在tomcat下运行

```bash
"C:\Program Files\Java\jdk-11.0.2\bin\java.exe" -XX:TieredStopAtLevel=1 -noverify -Dspring.output.ansi.enabled=always -Dcom.sun.management.jmxremote -Dspring.liveBeansView.mbeanDomain -Dspring.application.admin.enabled=true "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA 2018.3.4\lib\idea_rt.jar=11522:C:\Program Files\JetBrains\IntelliJ IDEA 2018.3.4\bin" -Dfile.encoding=UTF-8 -classpath C:\Users\pc\IdeaProjects\springboot03\target\classes;D:\maven\repository\org\springframework\boot\spring-boot-starter\2.1.3.RELEASE\spring-boot-starter-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot\2.1.3.RELEASE\spring-boot-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\spring-context\5.1.5.RELEASE\spring-context-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-autoconfigure\2.1.3.RELEASE\spring-boot-autoconfigure-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-logging\2.1.3.RELEASE\spring-boot-starter-logging-2.1.3.RELEASE.jar;D:\maven\repository\ch\qos\logback\logback-classic\1.2.3\logback-classic-1.2.3.jar;D:\maven\repository\ch\qos\logback\logback-core\1.2.3\logback-core-1.2.3.jar;D:\maven\repository\org\apache\logging\log4j\log4j-to-slf4j\2.11.2\log4j-to-slf4j-2.11.2.jar;D:\maven\repository\org\apache\logging\log4j\log4j-api\2.11.2\log4j-api-2.11.2.jar;D:\maven\repository\org\slf4j\jul-to-slf4j\1.7.25\jul-to-slf4j-1.7.25.jar;D:\maven\repository\javax\annotation\javax.annotation-api\1.3.2\javax.annotation-api-1.3.2.jar;D:\maven\repository\org\springframework\spring-core\5.1.5.RELEASE\spring-core-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-jcl\5.1.5.RELEASE\spring-jcl-5.1.5.RELEASE.jar;D:\maven\repository\org\yaml\snakeyaml\1.23\snakeyaml-1.23.jar;D:\maven\repository\org\springframework\boot\spring-boot-devtools\2.1.3.RELEASE\spring-boot-devtools-2.1.3.RELEASE.jar;D:\maven\repository\org\projectlombok\lombok\1.18.6\lombok-1.18.6.jar;D:\maven\repository\org\slf4j\slf4j-api\1.7.25\slf4j-api-1.7.25.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-web\2.1.3.RELEASE\spring-boot-starter-web-2.1.3.RELEASE.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-json\2.1.3.RELEASE\spring-boot-starter-json-2.1.3.RELEASE.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-databind\2.9.8\jackson-databind-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-annotations\2.9.0\jackson-annotations-2.9.0.jar;D:\maven\repository\com\fasterxml\jackson\core\jackson-core\2.9.8\jackson-core-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\datatype\jackson-datatype-jdk8\2.9.8\jackson-datatype-jdk8-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\datatype\jackson-datatype-jsr310\2.9.8\jackson-datatype-jsr310-2.9.8.jar;D:\maven\repository\com\fasterxml\jackson\module\jackson-module-parameter-names\2.9.8\jackson-module-parameter-names-2.9.8.jar;D:\maven\repository\org\springframework\boot\spring-boot-starter-tomcat\2.1.3.RELEASE\spring-boot-starter-tomcat-2.1.3.RELEASE.jar;D:\maven\repository\org\apache\tomcat\embed\tomcat-embed-core\9.0.16\tomcat-embed-core-9.0.16.jar;D:\maven\repository\org\apache\tomcat\embed\tomcat-embed-el\9.0.16\tomcat-embed-el-9.0.16.jar;D:\maven\repository\org\apache\tomcat\embed\tomcat-embed-websocket\9.0.16\tomcat-embed-websocket-9.0.16.jar;D:\maven\repository\org\hibernate\validator\hibernate-validator\6.0.14.Final\hibernate-validator-6.0.14.Final.jar;D:\maven\repository\javax\validation\validation-api\2.0.1.Final\validation-api-2.0.1.Final.jar;D:\maven\repository\org\jboss\logging\jboss-logging\3.3.2.Final\jboss-logging-3.3.2.Final.jar;D:\maven\repository\com\fasterxml\classmate\1.4.0\classmate-1.4.0.jar;D:\maven\repository\org\springframework\spring-web\5.1.5.RELEASE\spring-web-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-beans\5.1.5.RELEASE\spring-beans-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-webmvc\5.1.5.RELEASE\spring-webmvc-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-aop\5.1.5.RELEASE\spring-aop-5.1.5.RELEASE.jar;D:\maven\repository\org\springframework\spring-expression\5.1.5.RELEASE\spring-expression-5.1.5.RELEASE.jar cn.goingtodo.main.MainApplication

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)

2019-03-04 23:36:21.394  INFO 1636 --- [  restartedMain] cn.goingtodo.main.MainApplication        : Starting MainApplication on livingbody with PID 1636 (C:\Users\pc\IdeaProjects\springboot03\target\classes started by pc in C:\Users\pc\IdeaProjects\springboot03)
2019-03-04 23:36:21.394  INFO 1636 --- [  restartedMain] cn.goingtodo.main.MainApplication        : No active profile set, falling back to default profiles: default
2019-03-04 23:36:21.457  INFO 1636 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disable
2019-03-04 23:36:21.457  INFO 1636 --- [  restartedMain] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2019-03-04 23:36:23.020  INFO 1636 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2019-03-04 23:36:23.066  INFO 1636 --- [  restartedMain] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2019-03-04 23:36:23.066  INFO 1636 --- [  restartedMain] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.16]
2019-03-04 23:36:23.066  INFO 1636 --- [  restartedMain] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: [C:\Program Files\Java\jdk-11.0.2\bin;C:\WINDOWS\Sun\Java\bin;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\WINDOWS\System32\OpenSSH\;C:\Go\bin;C:\Program Files\nodejs\;C:\ProgramData\chocolatey\bin;C:\Program Files\Git\cmd;C:\Program Files\Intel\WiFi\bin\;C:\Program Files\Common Files\Intel\WirelessCommon\;C:\Program Files\Calibre2\;C:\Python37\Scripts\;C:\Python37\;C:\Users\pc\AppData\Local\Microsoft\WindowsApps;C:\Users\pc\go\bin;C:\Users\pc\AppData\Roaming\npm;C:\Users\pc\AppData\Local\atom\bin;C:\Program Files\Java\jdk-11.0.2\bin;;.]
2019-03-04 23:36:23.176  INFO 1636 --- [  restartedMain] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2019-03-04 23:36:23.176  INFO 1636 --- [  restartedMain] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1719 ms
2019-03-04 23:36:23.473  INFO 1636 --- [  restartedMain] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2019-03-04 23:36:23.629  INFO 1636 --- [  restartedMain] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2019-03-04 23:36:23.676  INFO 1636 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2019-03-04 23:36:23.692  INFO 1636 --- [  restartedMain] cn.goingtodo.main.MainApplication        : Started MainApplication in 2.751 seconds (JVM running for 3.906)
```