# Java12 的新特性

## 序

本文主要讲述一下 Java12 的新特性

## 版本号

```
java -version
openjdk version "12" 2019-03-19
OpenJDK Runtime Environment (build 12+33)
OpenJDK 64-Bit Server VM (build 12+33, mixed mode)
```

> 从 version 信息可以看出是 build 12+33

## 特性列表

- [189: Shenandoah: A Low-Pause-Time Garbage Collector (Experimental)](http://openjdk.java.net/jeps/189)

> Shenandoah GC 是一个面向 low-pause-time 的垃圾收集器，它最初由 Red Hat 实现，支持 aarch64 及 amd64 architecture；ZGC 也是面向 low-pause-time 的垃圾收集器，不过 ZGC 是基于 colored pointers 来实现，而 Shenandoah GC 是基于 brooks pointers 来实现；如果要使用 Shenandoah GC 需要编译时--with-jvm-features 选项带有 shenandoahgc，然后启动时使用-XX:+UnlockExperimentalVMOptions -XX:+UseShenandoahGC

- [230: Microbenchmark Suite](http://openjdk.java.net/jeps/230)

> 在 jdk 源码里头新增了一套基础的 microbenchmarks suite

- [325: Switch Expressions (Preview)](http://openjdk.java.net/jeps/325)

> 对 switch 进行了增强，除了使用 statement 还可以使用 expression，比如原来的写法如下：

```java
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```

现在可以改为如下写法：

```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
```

以及在表达式返回值

```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
```

对于需要返回值的 switch expression 要么正常返回值要么抛出异常，以下这两种写法都是错误的

```java
int i = switch (day) {
    case MONDAY -> {
        System.out.println("Monday"); 
        // ERROR! Block doesn't contain a break with value
    }
    default -> 1;
};
i = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY: 
        break 0;
    default: 
        System.out.println("Second half of the week");
        // ERROR! Group doesn't contain a break with value
};
```

- [334: JVM Constants API](http://openjdk.java.net/jeps/334)

> 新增了 JVM Constants API，具体来说就是 java.base 模块新增了 java.lang.constant 包，引入了 ConstantDesc 接口 (`ClassDesc、MethodTypeDesc、MethodHandleDesc 这几个接口直接继承了 ConstantDesc 接口`) 以及 Constable 接口；ConstantDesc 接口定义了 resolveConstantDesc 方法，Constable 接口定义了 describeConstable 方法；String、Integer、Long、Float、Double 均实现了这两个接口，而 EnumDesc 实现了 ConstantDesc 接口

- [340: One AArch64 Port, Not Two](http://openjdk.java.net/jeps/340)

> 64-bit Arm platform (arm64)，也可以称之为 aarch64；之前 JDK 有两个关于 aarch64 的实现，分别是 src/hotspot/cpu/arm 以及 open/src/hotspot/cpu/aarch64，它们的实现重复了，为了集中精力更好地实现 aarch64，该特性在源码中删除了 open/src/hotspot/cpu/arm 中关于 64-bit 的实现，保留其中 32-bit 的实现，于是 open/src/hotspot/cpu/aarch64 部分就成了 64-bit ARM architecture 的默认实现

- [341: Default CDS Archives](http://openjdk.java.net/jeps/341)

> java10 的新特性[JEP 310: Application Class-Data Sharing](http://openjdk.java.net/jeps/310) 扩展了 JDK5 引入的 Class-Data Sharing，支持 application 的 Class-Data Sharing；Class-Data Sharing 可以用于多个 JVM 共享 class，提升启动速度，最早只支持 system classes 及 serial GC，JDK9 对其进行扩展以支持 application classes 及其他 GC 算法，并在 JDK10 中开源出来 (`以前是 commercial feature`)；JDK11 将-Xshare:off 改为默认-Xshare:auto，以更加方便使用 CDS 特性；JDK12 的这个特性即在 64-bit 平台上编译 jdk 的时候就默认在${JAVA_HOME}/lib/server 目录下生成一份名为 classes.jsa 的默认 archive 文件 (`大概有 18M`) 方便大家使用

- [344: Abortable Mixed Collections for G1](http://openjdk.java.net/jeps/344)

> G1 在 garbage collection 的时候，一旦确定了 collection set(`CSet`) 开始垃圾收集这个过程是 without stopping 的，当 collection set 过大的时候，此时的 STW 时间会过长超出目标 pause time，这种情况在 mixed collections 时候比较明显。这个特性启动了一个机制，当选择了一个比较大的 collection set，允许将其分为 mandatory 及 optional 两部分 (`当完成 mandatory 的部分，如果还有剩余时间则会去处理 optional 部分`) 来将 mixed collections 从 without stopping 变为 abortable，以更好满足指定 pause time 的目标

- [346: Promptly Return Unused Committed Memory from G1](http://openjdk.java.net/jeps/346)

> G1 目前只有在 full GC 或者 concurrent cycle 的时候才会归还内存，由于这两个场景都是 G1 极力避免的，因此在大多数场景下可能不会及时会还 committed Java heap memory 给操作系统。JDK12 的这个特性新增了两个参数分别是 G1PeriodicGCInterval 及 G1PeriodicGCSystemLoadThreshold，设置为 0 的话，表示禁用。当上一次 garbage collection pause 过去 G1PeriodicGCInterval(`milliseconds`) 时间之后，如果 getloadavg()(`one-minute`) 低于 G1PeriodicGCSystemLoadThreshold 指定的阈值，则触发 full GC 或者 concurrent GC(`如果开启 G1PeriodicGCInvokesConcurrent`)，GC 之后 Java heap size 会被重写调整，然后多余的内存将会归还给操作系统

## 细项解读

上面列出的是大方面的特性，除此之外还有一些 api 的更新及废弃，主要见[JDK 12 Release Notes](http://jdk.java.net/12/release-notes)，这里举几个例子。

### 添加项

- 支持 unicode 11
- 支持 Compact Number Formatting

> 使用实例如下

```java
@Test
public void testCompactNumberFormat(){
    var cnf = NumberFormat.getCompactNumberInstance(Locale.CHINA, NumberFormat.Style.SHORT);
    System.out.println(cnf.format(1_0000));
    System.out.println(cnf.format(1_9200));
    System.out.println(cnf.format(1_000_000));
    System.out.println(cnf.format(1L << 30));
    System.out.println(cnf.format(1L << 40));
    System.out.println(cnf.format(1L << 50));
}
```

输出

```
1 万
2 万
100 万
11 亿
1 兆
1126 兆
```

- String 支持 transform、indent 操作

```java
@Test
public void testStringTransform(){
    System.out.println("hello".transform(new Function<String, Integer>() {
        @Override
        public Integer apply(String s) {
            return s.hashCode();
        }
    }));
}

@Test
public void testStringIndent(){
    System.out.println("hello".indent(3));
}
```

- Files 新增 mismatch 方法

```java
@Test
public void testFilesMismatch() throws IOException {
    FileWriter fileWriter = new FileWriter("/tmp/a.txt");
    fileWriter.write("a");
    fileWriter.write("b");
    fileWriter.write("c");
    fileWriter.close();

    FileWriter fileWriterB = new FileWriter("/tmp/b.txt");
    fileWriterB.write("a");
    fileWriterB.write("1");
    fileWriterB.write("c");
    fileWriterB.close();

    System.out.println(Files.mismatch(Path.of("/tmp/a.txt"),Path.of("/tmp/b.txt")));
}
```

- Collectors 新增 teeing 方法用于聚合两个 downstream 的结果

```java
@Test
public void testCollectorTeeing(){
    var result = Stream.of("Devoxx","Voxxed Days","Code One","Basel One")
            .collect(Collectors.teeing(Collectors.filtering(n -> n.contains("xx"),Collectors.toList()),
                    Collectors.filtering(n -> n.endsWith("One"),Collectors.toList()),
                    (List<String> list1, List<String> list2) -> List.of(list1,list2)
            ));

    System.out.println(result.get(0));
    System.out.println(result.get(1));
}
```

- CompletionStage 新增 exceptionallyAsync、exceptionallyCompose、exceptionallyComposeAsync 方法

```java
@Test
public void testExceptionallyAsync() throws ExecutionException, InterruptedException {
    LOGGER.info("begin");
    int result = CompletableFuture.supplyAsync(() -> {
        LOGGER.info("calculate");
        int i = 1/0;
        return 100;
    }).exceptionallyAsync((t) -> {
        LOGGER.info("error error:{}",t.getMessage());
        return 0;
    }).get();

    LOGGER.info("result:{}",result);
}
```

- JDK12 之前 CompletionStage 只有一个 exceptionally，该方法体在主线程执行，JDK12 新增了 exceptionallyAsync、exceptionallyComposeAsync 方法允许方法体在异步线程执行，同时新增了 exceptionallyCompose 方法支持在 exceptionally 的时候构建新的 CompletionStage
- Allocation of Old Generation of Java Heap on Alternate Memory Devices

> G1 及 Parallel GC 引入 experimental 特性，允许将 old generation 分配在诸如 NV-DIMM memory 的 alternative memory device

- ZGC: Concurrent Class Unloading

> ZGC 在 JDK11 的时候还不支持 class unloading，JDK12 对 ZGC 支持了 Concurrent Class Unloading，默认是开启，使用-XX:-ClassUnloading 可以禁用

- 新增-XX:+ExtensiveErrorReports

> -XX:+ExtensiveErrorReports 可以用于在 jvm crash 的时候收集更多的报告信息到 hs_err<pid>.log 文件中，product builds 中默认是关闭的，要开启的话，需要自己添加-XX:+ExtensiveErrorReports 参数

- 新增安全相关的改进

> 支持 java.security.manager 系统属性，当设置为 disallow 的时候，则不使用 SecurityManager 以提升性能，如果此时调用 System.setSecurityManager 则会抛出 UnsupportedOperationException
>  keytool 新增-groupname 选项允许在生成 key pair 的时候指定一个 named group
>  新增 PKCS12 KeyStore 配置属性用于自定义 PKCS12 keystores 的生成
>  Java Flight Recorder 新增了 security-related 的 event
>  支持 ChaCha20 and Poly1305 TLS Cipher Suites

- jdeps Reports Transitive Dependences

> jdeps 的--print-module-deps, --list-deps, 以及--list-reduce-deps 选项得到增强，新增--no-recursive 用于 non-transitive 的依赖分析，--ignore-missing-deps 用于 suppress missing dependence errors

### 移除项

- 移除 com.sun.awt.SecurityWarnin
- 移除 FileInputStream、FileOutputStream、Java.util.ZipFile/Inflator/Deflator 的 finalize 方法
- 移除 GTE CyberTrust Global Root
- 移除 javac 的-source, -target 对 6 及 1.6 的支持，同时移除--release 选项

### 废弃项

- 废弃的 API 列表见[deprecated-list](http://cr.openjdk.java.net/~iris/se/12/latestSpec/api/deprecated-list.html) 
- 废弃-XX:+/-MonitorInUseLists 选项
- 废弃 Default Keytool 的-keyalg 值

### 已知问题

- Swing 不支持 GTK+ 3.20 及以后的版本
- 在使用 JVMCI Compiler(`比如 Graal`) 的时候，JVMTI 的 can_pop_frame 及 can_force_early_return 的 capabilities 是被禁用的

### 其他事项

- 如果用户没有指定 user.timezone 且从操作系统获取的为空，那么 user.timezone 属性的初始值为空变为 null
- java.net.URLPermission 的行为发生轻微变化，以前它会忽略 url 中的 query 及 fragment 部分，这次改动新增 query 及 fragment 部分，即`scheme : // authority [ / path ]`变动为`scheme : // authority [ / path ] [ ignored-query-or-fragment ]` 
- javax.net.ssl.SSLContext API 及 Java Security Standard Algorithm Names 规范移除了必须实现 TLSv1 及 TLSv1.1 的规定

## 小结

- java12 不是 LTS(`Long-Term Support`) 版本 (`oracle 版本才有 LTS`)，oracle 对该版本的 support 周期为 6 个月。这个版本主要有几个更新点，一个是语法层更新，一个是 API 层面的更新，另外主要是 GC 方面的更新。
- 语法层面引入了 preview 版本的 Switch Expressions；API 层面引入了 JVM Constants API，引入 CompactNumberFormat，让 NumberFormat 支持 COMPACTSTYLE，对 String、Files、Collectors、CompletionStage 等新增方法；GC 方面引入了 experimental 版本的 Shenandoah GC，不过 oracle build 的 openjdk 没有 enable Shenandoah GC support；另外主要对 ZGC 及 G1 GC 进行了改进
- 其中 JDK12 对 ZGC 支持了 Concurrent Class Unloading，默认是开启，使用-XX:-ClassUnloading 可以禁用；对于 G1 GC 则新增支持 Abortable Mixed Collections 以及 Promptly Return Unused Committed Memory 特性

## doc

- [openjdk 12](http://openjdk.java.net/projects/jdk/12/)
- [JDK 12 Release Notes](http://jdk.java.net/12/release-notes)
- [Java 12 Released with Experimental Switch Expressions and Shenandoah GC](https://www.infoq.com/news/2019/03/java12-released)
- [Definitive Guide To Java 12](https://blog.codefx.org/java/java-12-guide/)
- [Definitive Guide To Switch Expressions In Java 12](https://blog.codefx.org/java/switch-expressions/)
- [JVM Class Data Sharing](https://kupczynski.info/2018/05/29/jvm-class-data-sharing.html)
- [JEP 310: Application Class-Data Sharing](http://openjdk.java.net/jeps/310)
- [Improve Launch Times On Java 10 With Application Class-Data Sharing](https://blog.codefx.org/java/application-class-data-sharing/)
- [Make -Xshare:auto the default for server VM](https://bugs.openjdk.java.net/browse/JDK-8197967)
- [Using application class-data sharing](https://subscription.packtpub.com/book/application_development/9781789132359/1/ch01lvl1sec16/using-application-class-data-sharing)
- [Java Performance Tuning News February 2018](http://www.javaperformancetuning.com/news/news219.shtml)
- [JDK 12 Security Enhancements](https://seanjmullan.org/blog/2019/03/19/jdk12)