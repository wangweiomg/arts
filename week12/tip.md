## ARTS - Tip
## 关于linux tomcat启动war工程的一个log4j报错

### 背景
在进行linux centos6.5部署war工程时候，报了一个错导致启动不起来，但是在开发时候在idea里完全没问题。

### 问题现象

启动时候catalina.out报错，如下：

```
09-Oct-2018 15:04:39.725 INFO [Abandoned connection cleanup thread] org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading Illegal access: this web application instance has been stopped already. Could not load []. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.
 java.lang.IllegalStateException: Illegal access: this web application instance has been stopped already. Could not load []. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.
	at org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading(WebappClassLoaderBase.java:1329)
	at org.apache.catalina.loader.WebappClassLoaderBase.getResource(WebappClassLoaderBase.java:1004)
	at com.mysql.jdbc.AbandonedConnectionCleanupThread.checkContextClassLoaders(AbandonedConnectionCleanupThread.java:90)
	at com.mysql.jdbc.AbandonedConnectionCleanupThread.run(AbandonedConnectionCleanupThread.java:63)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
```

这里报错描述比较粗略，具体看localhost.xxxx.log 信息如下：

```
9-Oct-2018 15:04:31.958 SEVERE [localhost-startStop-1] org.apache.catalina.core.StandardContext.listenerStart Exception sending context initialized event to listener instance of class [ch.qos.logback.ext.spring.web.LogbackConfigListener]
 java.lang.ClassCastException: org.slf4j.impl.Log4jLoggerFactory cannot be cast to ch.qos.logback.classic.LoggerContext
        at ch.qos.logback.ext.spring.LogbackConfigurer.initLogging(LogbackConfigurer.java:72)
        at ch.qos.logback.ext.spring.web.WebLogbackConfigurer.initLogging(WebLogbackConfigurer.java:142)
        at ch.qos.logback.ext.spring.web.LogbackConfigListener.contextInitialized(LogbackConfigListener.java:54)
        at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4792)
        
        
        
       
09-Oct-2018 15:04:36.021 SEVERE [localhost-startStop-1] org.apache.catalina.core.StandardContext.listenerStop Exception sending context destroyed event to listener instance of class [ch.qos.logback.ext.spring.web.LogbackConfigListener]
 java.lang.NullPointerException
        at ch.qos.logback.ext.spring.LogbackConfigurer.shutdownLogging(LogbackConfigurer.java:104)
        at ch.qos.logback.ext.spring.web.WebLogbackConfigurer.shutdownLogging(WebLogbackConfigurer.java:187)
        at ch.qos.logback.ext.spring.web.LogbackConfigListener.contextDestroyed(LogbackConfigListener.java:49
```

### 分析

根据报错，初步定为是logback 初始化的问题，检查了配置和jar包后没法现问题，就直接看本地的idea里启动日志，里面有警告日志如下：


```
09-Oct-2018 15:10:12.019 信息 [RMI TCP Connection(2)-127.0.0.1] org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/Users/wangwei/Workspace/credit-center/credit-webapp/target/credit/WEB-INF/lib/logback-classic-1.1.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/Users/wangwei/Workspace/credit-center/credit-webapp/target/credit/WEB-INF/lib/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [ch.qos.logback.classic.util.ContextSelectorStaticBinder]
```

注意是 multiple SLF4j bindings ， 说明slf4j 的实现有log4j 还有logback两者冲突了，怀疑是这个问题，就使用maven-war-plugin打包插件，排除掉log4j的jar文件，配置如下：

```
<plugins>
            <plugin>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.2</version>
                <configuration>
                    <packagingExcludes>WEB-INF/lib/*log4j*.jar</packagingExcludes>
                </configuration>
            </plugin>
        </plugins>
```

编译打包重新启动，发现问题结局， 

猜测这个问题可能是Linux校验严格。


