# **Java 11 的新特性**

## 序

本文主要讲述一下 Java11 的新特性

## 版本号

```
java -version
openjdk version "11" 2018-09-25
OpenJDK Runtime Environment 18.9 (build 11+28)
OpenJDK 64-Bit Server VM 18.9 (build 11+28, mixed mode)
```

- General-Availability Release 版本是基于 tag 为 jdk-11+28 的版本编译
- 从 version 信息可以看出是 build 11+28

## 特性列表

- [181: Nest-Based Access Control](https://openjdk.java.net/jeps/181)

> 相关解读 [Java Nestmate 稳步推进](http://www.infoq.com/cn/news/2018/03/Nestmates)，[Specification for JEP 181: Nest-based Access Control](http://cr.openjdk.java.net/~dlsmith/nestmates.html)
> 简单的理解就是 Class 类新增了 getNestHost，getNestMembers 方法

- [309: Dynamic Class-File Constants](https://openjdk.java.net/jeps/309)

> 相关解读 [Specification for JEP 309: Dynamic Class-File Constants (JROSE EDITS)](http://cr.openjdk.java.net/~jrose/jvm/constant-dynamic-jrose.html)
> jvm 规范里头对 Constant pool 新增一类 CONSTANT_Dynamic

- [315: Improve Aarch64 Intrinsics](https://openjdk.java.net/jeps/315)

> 对于 AArch64 处理器改进现有的 string、array 相关函数，并新实现 java.lang.Math 的 sin、cos、log 方法

- [318: Epsilon: A No-Op Garbage Collector](https://openjdk.java.net/jeps/318)

> 引入名为 Epsilon 的垃圾收集器，该收集器不做任何垃圾回收，可用于性能测试、短生命周期的任务等，使用 - XX:+UseEpsilonGC 开启

- [320: Remove the Java EE and CORBA Modules](https://openjdk.java.net/jeps/320)(`重磅`)

> 将 java9 标记废弃的 Java EE 及 CORBA 模块移除掉，具体如下：
> 1. xml 相关的，java.xml.ws, java.xml.bind，java.xml.ws，java.xml.ws.annotation，jdk.xml.bind，jdk.xml.ws 被移除，只剩下 java.xml，java.xml.crypto,jdk.xml.dom 这几个模块；
> 2. java.corba，java.se.ee，java.activation，java.transaction 被移除，但是 java11 新增一个 java.transaction.xa 模块

- [321: HTTP Client (Standard)](https://openjdk.java.net/jeps/321)(`重磅`)

> 相关解读 [java9 系列 (六)HTTP/2 Client (Incubator)](https://segmentfault.com/a/1190000013518969)，[HTTP Client Examples and Recipes](https://openjdk.java.net/groups/net/httpclient/recipes.html)，在 java9 及 10 被标记 incubator 的模块 jdk.incubator.httpclient，在 java11 被标记为正式，改为 java.net.http 模块。

- [323: Local-Variable Syntax for Lambda Parameters](https://openjdk.java.net/jeps/323)

> 相关解读 [New Java 11 Language Feature: Local-Variable Type Inference (var) extended to Lambda Expression Parameters](https://medium.com/the-java-report/java-11-sneak-peek-local-variable-type-inference-var-extended-to-lambda-expression-parameters-e31e3338f1fe)
> 允许 lambda 表达式使用 var 变量，比如 (var x, var y) -> x.process(y)，如果仅仅是这样写，倒是无法看出写 var 有什么优势而且反而觉得有点多此一举，但是如果要给 lambda 表达式变量标注注解的话，那么这个时候 var 的作用就突显出来了 (@Nonnull var x, @Nullable var y) -> x.process(y)

- [324: Key Agreement with Curve25519 and Curve448](https://openjdk.java.net/jeps/324)

> 使用 RFC 7748 中描述的 Curve25519 和 Curve448 实现 key agreement

- [327: Unicode 10](https://openjdk.java.net/jeps/327)

> 升级现有的 API，支持 Unicode10.0.0

- [328: Flight Recorder](https://openjdk.java.net/jeps/328)

> 相关解读 [Java 11 Features: Java Flight Recorder](https://dzone.com/articles/java-11-features-java-flight-recorder)
> Flight Recorder 以前是商业版的特性，在 java11 当中开源出来，它可以导出事件到文件中，之后可以用 Java Mission Control 来分析。可以在应用启动时配置 java -XX:StartFlightRecording，或者在应用启动之后，使用 jcmd 来录制，比如
> ```
> $ jcmd <pid> JFR.start
> $ jcmd <pid> JFR.dump filename=recording.jfr
> $ jcmd <pid> JFR.stop
> ```

- [329: ChaCha20 and Poly1305 Cryptographic Algorithms](https://openjdk.java.net/jeps/329)

> 实现 RFC 7539 的 ChaCha20 and ChaCha20-Poly1305 加密算法

- [330: Launch Single-File Source-Code Programs](https://openjdk.java.net/jeps/330)(`重磅`)

> 相关解读 [Launch Single-File Source-Code Programs in JDK 11](https://dzone.com/articles/launch-single-file-source-code-programs-in-jdk-11)
> 有了这个特性，可以直接 java HelloWorld.java 来执行 java 文件了，无需先 javac 编译为 class 文件然后再 java 执行 class 文件，两步合成一步

- [331: Low-Overhead Heap Profiling](https://openjdk.java.net/jeps/331)

> 通过 JVMTI 的 SampledObjectAlloc 回调提供了一个开销低的 heap 分析方式

- [332: Transport Layer Security (TLS) 1.3](https://openjdk.java.net/jeps/332)(`重磅`)

> 支持 RFC 8446 中的 TLS 1.3 版本

- [333: ZGC: A Scalable Low-Latency Garbage Collector(Experimental)](https://openjdk.java.net/jeps/333)(`重磅`)

> 相关解读 [JDK11 的 ZGC 小试牛刀](https://segmentfault.com/a/1190000015725327)，[一文读懂 Java 11 的 ZGC 为何如此高效](https://mp.weixin.qq.com/s/nAjPKSj6rqB_eaqWtoJsgw)

- [335: Deprecate the Nashorn JavaScript Engine](https://openjdk.java.net/jeps/335)

> 相关解读 [Oracle 弃用 Nashorn JavaScript 引擎](http://www.infoq.com/cn/news/2018/06/deprecate-nashorn)，[Oracle GraalVM announces support for Nashorn migration](https://medium.com/graalvm/oracle-graalvm-announces-support-for-nashorn-migration-c04810d75c1f)
> 废除 Nashorn javascript 引擎，在后续版本准备移除掉，有需要的可以考虑使用 GraalVM

- [336: Deprecate the Pack200 Tools and API](https://openjdk.java.net/jeps/336)

> 废除了 pack200 以及 unpack200 工具以及 java.util.jar 中的 Pack200 API。Pack200 主要是用来压缩 jar 包的工具，不过由于网络下载速度的提升以及 java9 引入模块化系统之后不再依赖 Pack200，因此这个版本将其移除掉。

## 细项解读

上面列出的是大方面的特性，除此之外还有一些 api 的更新及废弃，主要见 [What's New in JDK 11 - New Features and Enhancements](https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html#NewFeature) 以及 [90 New Features (and APIs) in JDK 11](https://www.azul.com/90-new-features-and-apis-in-jdk-11/)，这里举几个例子。

### 添加项

- Collection.toArray(IntFunction)

```java
@Test
public void testCollectionToArray(){
    Set<String> names = Set.of("Fred", "Wilma", "Barney", "Betty");
    String[] copy = new String[names.size()];
    names.toArray(copy);
    System.out.println(Arrays.toString(copy));
    System.out.println(Arrays.toString(names.toArray(String[]::new)));
}
```

> Collection 类新增 toArray(IntFunction) 的 default 方法，可以直接通过传入 IntFunction 告知要转换的目标类型

- String.strip

```java
@Test
public void testStrip(){
    String text = "  u2000a  b  ";
    Assert.assertEquals("a  b",text.strip());
    Assert.assertEquals("u2000a  b",text.trim());
    Assert.assertEquals("a  b  ",text.stripLeading());
    Assert.assertEquals("  u2000a  b",text.stripTrailing());
}
```

> Java 11 对 String 类新增了 strip，stripLeading 以及 stripTrailing 方法，除了 strip 相关的方法还新增了 isBlank、lines、repeat(int) 等方法

- 添加了 Google Trust Services GlobalSign Root Certificates
- 添加了 GoDaddy Root Certificates
- 添加了 T-Systems, GlobalSign and Starfield Services Root Certificates
- 添加了 Entrust Root Certificates

### 移除项

- 移除了 com.sun.awt.AWTUtilities
- 移除了 sun.misc.Unsafe.defineClass，使用 java.lang.invoke.MethodHandles.Lookup.defineClass 来替代
- 移除了 Thread.destroy() 以及 Thread.stop(Throwable) 方法
- 移除了 sun.nio.ch.disableSystemWideOverlappingFileLockCheck、sun.locale.formatasdefault 属性
- 移除了 jdk.snmp 模块
- 移除了 javafx，openjdk 估计是从 java10 版本就移除了，oracle jdk10 还尚未移除 javafx，而 java11 版本则 oracle 的 jdk 版本也移除了 javafx
- 移除了 Java Mission Control，从 JDK 中移除之后，需要自己单独下载
- 移除了这些 Root Certificates ：Baltimore Cybertrust Code Signing CA，SECOM ，AOL and Swisscom

### 废弃项

- 废弃了 Nashorn JavaScript Engine
- 废弃了 - XX+AggressiveOpts 选项
- -XX:+UnlockCommercialFeatures 以及 - XX:+LogCommercialFeatures 选项也不再需要
- 废弃了 Pack200 工具及其 API

## 小结

- java11 是 java 改为 6 个月发布一版的策略之后的第一个 LTS(`Long-Term Support`) 版本 (`oracle版本才有LTS`)，这个版本最主要的特性是：在模块方面移除 Java EE 以及 CORBA 模块，在 JVM 方面引入了实验性的 ZGC，在 API 方面正式提供了 HttpClient 类。
- 从 java11 版本开始，不再单独发布 JRE 或者 Server JRE 版本了，有需要的可以自己通过 jlink 去定制 runtime image

## doc

- [JDK11](https://jdk.java.net/11)
- [JDK11 Features](https://openjdk.java.net/projects/jdk/11/)
- [Introducing Java SE 11](https://blogs.oracle.com/java-platform-group/introducing-java-se-11)(`官方解读`)
- [JDK 11 Release Notes](https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html)(`官方细项解读`)
- [What is new in Java 11 ?](https://medium.com/antelabs/what-is-new-in-java-11-442af9315f07)
- [Java Nestmate 稳步推进](http://www.infoq.com/cn/news/2018/03/Nestmates)
- [Specification for JEP 181: Nest-based Access Control](http://cr.openjdk.java.net/~dlsmith/nestmates.html)
- [Specification for JEP 309: Dynamic Class-File Constants (JROSE EDITS)](http://cr.openjdk.java.net/~jrose/jvm/constant-dynamic-jrose.html)
- [Java 11 Features: Java Flight Recorder](https://dzone.com/articles/java-11-features-java-flight-recorder)
- [java9 系列 (六)HTTP/2 Client (Incubator)](https://segmentfault.com/a/1190000013518969)
- [Java 11: Standardized HTTP Client API](https://dzone.com/articles/java-11-standardized-http-client-api)
- [java.net.http javadoc](https://cr.openjdk.java.net/~chegar/httpclient/02/javadoc/api/java.net.http/module-summary.html)
- [HTTP Client Examples and Recipes](https://openjdk.java.net/groups/net/httpclient/recipes.html)
- [New Java 11 Language Feature: Local-Variable Type Inference (var) extended to Lambda Expression Parameters](https://medium.com/the-java-report/java-11-sneak-peek-local-variable-type-inference-var-extended-to-lambda-expression-parameters-e31e3338f1fe)
- [JDK11 的 ZGC 小试牛刀](https://segmentfault.com/a/1190000015725327)
- [一文读懂 Java 11 的 ZGC 为何如此高效](https://mp.weixin.qq.com/s/nAjPKSj6rqB_eaqWtoJsgw)
- [Oracle 弃用 Nashorn JavaScript 引擎](http://www.infoq.com/cn/news/2018/06/deprecate-nashorn)
- [Oracle GraalVM announces support for Nashorn migration](https://medium.com/graalvm/oracle-graalvm-announces-support-for-nashorn-migration-c04810d75c1f)
- [JDK 11: New Default Collection Method toArray(IntFunction)](http://marxsoftware.blogspot.com/2018/07/jdk-11-collection-toArray-IntFunction.html)
- [90 New Features (and APIs) in JDK 11](https://www.azul.com/90-new-features-and-apis-in-jdk-11/)
- [APIs To Be Removed from Java 11](http://marxsoftware.blogspot.com/2018/08/apis-to-be-removed-from-java-11.html)
- [Java 11 String API Updates](https://4comprehension.com/java-11-string-api-updates/)