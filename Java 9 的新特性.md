# Java 9 的新特性

## 序

本文主要讲述一下 Java9 的新特性

## 特性列表

完整的特性详见 [JDK 9 features](http://openjdk.java.net/projects/jdk9/)，这里列几个相对重要的：

### 模块系统 JPMS(`重磅`)

相关的规范及 JEP:

- [Java Platform Module System (JSR 376)](http://openjdk.java.net/projects/jigsaw/spec/)
- [JEP 261: Module System](http://openjdk.java.net/jeps/261)
- [JEP 200: The Modular JDK](http://openjdk.java.net/jeps/200)
- [JEP 201: Modular Source Code](http://openjdk.java.net/jeps/201)
- [JEP 282: jlink: The Java Linker](http://openjdk.java.net/jeps/282)
- [JEP 220: Modular Run-Time Images](http://openjdk.java.net/jeps/220)
- [JEP 260: Encapsulate Most Internal APIs](http://openjdk.java.net/jeps/260)

相关解读

- [java9 系列 (三) 模块系统精要](https://segmentfault.com/a/1190000013357446)
- [java9 opens 与 exports 的区别](https://segmentfault.com/a/1190000013409571)
- [java9 迁移注意事项](https://segmentfault.com/a/1190000013398709)
- [java9 module 相关选项解析](https://segmentfault.com/a/1190000013440386)

### G1 成为默认垃圾回收器

相关 JEP:

- [JEP 248: Make G1 the Default Garbage Collector](http://openjdk.java.net/jeps/248)
- [JEP 291: Deprecate the Concurrent Mark Sweep (CMS) Garbage Collector](http://openjdk.java.net/jeps/291)
- [JEP 278: Additional Tests for Humongous Objects in G1](http://openjdk.java.net/jeps/278)

相关解读

- [java9 系列 (九)Make G1 the Default Garbage Collector](https://segmentfault.com/a/1190000013615459)

### Unified JVM/GC Logging

相关 JEP:

- [JEP 158: Unified JVM Logging](http://openjdk.java.net/jeps/158)
- [JEP 264: Platform Logging API and Service](http://openjdk.java.net/jeps/264)
- [JEP 271: Unified GC Logging](http://openjdk.java.net/jeps/271)

相关解读

- [java9 gc log 参数迁移](https://segmentfault.com/a/1190000013475524)

### HTTP/2 Client(Incubator)

支持 HTTP2，同时改进 httpclient 的 api，支持异步模式。

相关 JEP

- [JEP 110: HTTP/2 Client (Incubator)](http://openjdk.java.net/jeps/110)

相关解读

- [java9 系列 (六)HTTP/2 Client (Incubator)](https://segmentfault.com/a/1190000013518969)

### jshell: The Java Shell (Read-Eval-Print Loop)

相关 JEP

- [JEP 222: jshell: The Java Shell (Read-Eval-Print Loop)](http://openjdk.java.net/jeps/222)

相关解读

- [java9 系列 (一) 安装及 jshell 使用](https://segmentfault.com/a/1190000011321448)

### Convenience Factory Methods for Collections

相关 JEP

- [JEP 269: Convenience Factory Methods for Collections](http://openjdk.java.net/jeps/269)

以前大多使用 Guava 类库集合类的工厂，比如

```
Lists.newArrayList(1,2,3,4,5);
Sets.newHashSet(1,2,3,4,5);
Maps.newHashMap();
```

注意，上面这种返回的集合是 mutable 的

现在 java9 可以直接利用 jdk 内置的集合工厂，比如

```
List.of(1,2,3,4,5);
Set.of(1,2,3,4,5);
Map.of("key1","value1","key2","value2","key3","value3");
```

注意，jdk9 上面这种集合工厂返回的是 immutable 的

### Process API Updates

相关 JEP

- [JEP 102: Process API Updates](http://openjdk.java.net/jeps/102)

相关解读

- [java9 系列 (四)Process API 更新](https://segmentfault.com/a/1190000013496056)

### Stack-Walking API

相关 JEP

- [JEP 259: Stack-Walking API](http://openjdk.java.net/jeps/259)

相关解读

- [java9 系列 (五)Stack-Walking API](https://segmentfault.com/a/1190000013506140)

### Variable Handles

相关 JEP

- [JEP 193: Variable Handles](http://openjdk.java.net/jeps/193)

相关解读

- [java9 系列 (七)Variable Handles](https://segmentfault.com/a/1190000013544841)

### docker 方面支持

- [Java SE support for Docker CPU and memory limits](https://blogs.oracle.com/java-platform-group/java-se-support-for-docker-cpu-and-memory-limits)
- [Docker CPU limits](https://bugs.openjdk.java.net/browse/JDK-8140793)
- [Experimental support for Docker memory limits](https://bugs.openjdk.java.net/browse/JDK-8170888)
- [Docker memory limits](https://bugs.openjdk.java.net/browse/JDK-8146115)

### 其他

- [JEP 238: Multi-Release JAR Files](http://openjdk.java.net/jeps/238)
  - [java9 系列 (八)Multi-Release JAR Files](https://segmentfault.com/a/1190000013584354)
- [JEP 266: More Concurrency Updates](http://openjdk.java.net/jeps/266)
- [JEP 274: Enhanced Method Handles](http://openjdk.java.net/jeps/274)
- [JEP 295: Ahead-of-Time Compilation](http://openjdk.java.net/jeps/295)

## 小结

java9 大刀阔斧，重磅引入了模块化系统，自身 jdk 的类库也首当其冲模块化。新引入的 jlink 可以精简化 jdk 的大小，外加 Alpine Linux 的 docker 镜像，可以大大减少 java 应用的 docker 镜像大小，同时也支持了 Docker 的 cpu 和 memory 限制 (`Java SE 8u131及以上版本开始支持`)，非常值得使用。

## doc

- [JDK 9 features](http://openjdk.java.net/projects/jdk9/)
- [Java 9 新特性概述](https://www.ibm.com/developerworks/cn/java/the-new-features-of-Java-9/index.html)
- [java9 系列 (一) 安装及 jshell 使用](https://segmentfault.com/a/1190000011321448)
- [java9 系列 (二)docker 运行 java9](https://segmentfault.com/a/1190000011331367)
- [java9 系列 (三) 模块系统精要](https://segmentfault.com/a/1190000013357446)
- [java9 系列 (四)Process API 更新](https://segmentfault.com/a/1190000013496056)
- [java9 系列 (五)Stack-Walking API](https://segmentfault.com/a/1190000013506140)
- [java9 系列 (六)HTTP/2 Client (Incubator)](https://segmentfault.com/a/1190000013518969)
- [java9 系列 (七)Variable Handles](https://segmentfault.com/a/1190000013544841)
- [java9 系列 (八)Multi-Release JAR Files](https://segmentfault.com/a/1190000013584354)
- [java9 系列 (九)Make G1 the Default Garbage Collector](https://segmentfault.com/a/1190000013615459)
- [java9 opens 与 exports 的区别](https://segmentfault.com/a/1190000013409571)
- [java9 迁移注意事项](https://segmentfault.com/a/1190000013398709)
- [java9 gc log 参数迁移](https://segmentfault.com/a/1190000013475524)
- [java9 module 相关选项解析](https://segmentfault.com/a/1190000013440386)
- [使用 maven 构建 java9 service 实例](https://segmentfault.com/a/1190000013371882)
- [使用示例带你提前了解 Java 9 中的新特性](http://yifeng.studio/2017/03/12/translation-java-9-features-with-examples/)