## ARTS - Tip 补11.19
## Maven 构建的意外killed

### 问题场景
在linux环境构建java web项目时候，运行 ```mvn clean package``` 后，会意外killed， 

```
[INFO] Packaging webapp
[INFO] Processing war project
[INFO] Webapp assembled in [1156 msecs]
[INFO] Building war: /home/jack/git/xxx/zzz/target/x.war

[1]  + 25650 killed     mvn clean package
```

### 问题定位

加上-X 开启debug模式

```
INFO] Copying xstream-1.4.7.jar to /home/jack/git/credit-c[1]    25909 killed     mvn package -X
```

没有报错，还是被killed, 可能是被操作系统killed的，只好谷歌了。

在这里找到了一点描述
[Maven compilation dies with “Killed”](https://stackoverflow.com/questions/7278514/maven-compilation-dies-with-killed)

说可能引起了OOM，所以系统把它kill 了，觉得这种可信度很大，于是就往调整内存那里处理。

### 尝试解决

然后查到资料说可以在编译插件设置编译最大最小内存

根据官网 [Compile Using Memory Allocation Enhancements
](https://maven.apache.org/plugins/maven-compiler-plugin/examples/compile-with-memory-enhancements.html)
于是设置如下：

```
<!-- 编译插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                    <fork>true</fork>
                    <meminitial>1024m</meminitial>
                    <maxmem>2048m</maxmem>
                </configuration>
            </plugin>

```
结果编译报错了

```
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.5.1:compile (default-compile) on project credit-webapp: Compilation failure -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.5.1:compile (default-compile) on project credit-webapp: Compilation failure
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:215)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:156)
```

继续找资料，发现这个回答 [Maven Out of Memory Build Failure](https://stackoverflow.com/questions/12498738/maven-out-of-memory-build-failure)

>What kind of 'web' module are you talking about? Is it a simple war and has >packaging type war?

>If you are not using Google's web toolkit (GWT) then you don't need to offer >any gwt.extraJvmArgs

>Forking the compile process might be not the best idea because then you start >a second process which ignores the MAVEN_OPTS at all and makes analysis more >difficult.

>So I would try to increase the Xmx by setting the MAVEN_OPTS

>```export MAVEN_OPTS="-Xmx3000m"```
> 
> And don't fork the compiler to a different process

说不用分开编译，因为会造成分析困难, 采用他的方式


于是就设置了

```
export MAVEN_OPTS="-Xmx1024m

```
问题解决。


