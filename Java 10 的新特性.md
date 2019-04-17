# Java 10 的新特性

## 序

本文主要讲述一下 Java10 的新特性

## 特性列表

- [286: Local-Variable Type Inference](http://openjdk.java.net/jeps/286)(`重磅`)

> 相关解读: [java10 系列 (二)Local-Variable Type Inference](https://segmentfault.com/a/1190000014025792)

- [296: Consolidate the JDK Forest into a Single Repository](http://openjdk.java.net/jeps/296)
- [304: Garbage-Collector Interface](http://openjdk.java.net/jeps/304)
- [307: Parallel Full GC for G1](http://openjdk.java.net/jeps/307)
- [310: Application Class-Data Sharing](http://openjdk.java.net/jeps/310)
- [312: Thread-Local Handshakes](http://openjdk.java.net/jeps/312)
- [313: Remove the Native-Header Generation Tool (javah)](http://openjdk.java.net/jeps/313)
- [314: Additional Unicode Language-Tag Extensions](http://openjdk.java.net/jeps/314)
- [316: Heap Allocation on Alternative Memory Devices](http://openjdk.java.net/jeps/316)
- [317: Experimental Java-Based JIT Compiler](http://openjdk.java.net/jeps/317)(`重磅`)

> 相关解读: [Java10 来了，来看看它一同发布的全新 JIT 编译器](https://mp.weixin.qq.com/s/NyDANTzK_uv6hwTXjknDLw)

- [319: Root Certificates](http://openjdk.java.net/jeps/319)

> 相关解读: [OpenJDK 10 Now Includes Root CA Certificates](https://dzone.com/articles/openjdk-10-now-includes-root-ca-certificates)

- [322: Time-Based Release Versioning](http://openjdk.java.net/jeps/322)

> 相关解读: [java10 系列 (一)Time-Based Release Versioning](https://segmentfault.com/a/1190000013885784)

## 细项解读

上面列出的是大方面的特性，除此之外还有一些 api 的更新及废弃，主要见 [What's New in JDK 10 - New Features and Enhancements](http://www.oracle.com/technetwork/java/javase/10-relnote-issues-4108729.html)，这里举几个例子。

### Optional.orElseThrow()

/Library/Java/JavaVirtualMachines/jdk-10.jdk/Contents/Home/lib/src.zip!/java.base/java/util/Optional.java

- 源码

```java
/**
 * If a value is present, returns the value, otherwise throws
 * {@code NoSuchElementException}.
 *
 * @return the non-{@code null} value described by this {@code Optional}
 * @throws NoSuchElementException if no value is present
 * @since 10
 */
public T orElseThrow() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}
```

- 实例

```java
@Test
public void testOrElseThrow(){
    var data = List.of("a","b","c");
    Optional<String> optional = data.stream()
            .filter(s -> s.startsWith("z"))
            .findAny();
    String res = optional.orElseThrow();
    System.out.println(res);
}
```

> 新增了 orElseThrow 与 get 相对应

输出

```shell
java.util.NoSuchElementException: No value present
    at java.base/java.util.Optional.orElseThrow(Optional.java:371)
    at com.example.FeatureTest.testOrElseThrow(FeatureTest.java:19)
    at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.base/java.lang.reflect.Method.invoke(Method.java:564)
    at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
    at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
    at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
    at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
    at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
    at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
    at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
    at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
    at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
    at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
    at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
    at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
    at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
    at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
    at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
    at com.intellij.rt.execution.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:47)
    at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:242)
    at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:70)
```

### APIs for Creating Unmodifiable Collections

java9 新增的 of 工厂方法的接口参数是一个个元素，java10 新增 List.copyOf，Set.copyOf 及 Map.copyOf 用来从已有集合创建 ImmutableCollections

- List.copyOf 源码

```java
/**
 * Returns an <a href="#unmodifiable">unmodifiable List</a> containing the elements of
 * the given Collection, in its iteration order. The given Collection must not be null,
 * and it must not contain any null elements. If the given Collection is subsequently
 * modified, the returned List will not reflect such modifications.
 *
 * @implNote
 * If the given Collection is an <a href="#unmodifiable">unmodifiable List</a>,
 * calling copyOf will generally not create a copy.
 *
 * @param <E> the {@code List}'s element type
 * @param coll a {@code Collection} from which elements are drawn, must be non-null
 * @return a {@code List} containing the elements of the given {@code Collection}
 * @throws NullPointerException if coll is null, or if it contains any nulls
 * @since 10
 */
@SuppressWarnings("unchecked")
static <E> List<E> copyOf(Collection<? extends E> coll) {
    if (coll instanceof ImmutableCollections.AbstractImmutableList) {
        return (List<E>)coll;
    } else {
        return (List<E>)List.of(coll.toArray());
    }
}
```

- 实例

```java
@Test(expected = UnsupportedOperationException.class)
public void testCollectionCopyOf(){
    List<String> list = IntStream.rangeClosed(1,10)
            .mapToObj(i -> "num"+i)
            .collect(Collectors.toList());
    List<String> newList = List.copyOf(list);
    newList.add("not allowed");
}
```

> Collectors 新增了 toUnmodifiableList， toUnmodifiableSet 以及 toUnmodifiableMap 方法

```java
@Test(expected = UnsupportedOperationException.class)
public void testCollectionCopyOf(){
    List<String> list = IntStream.rangeClosed(1,10)
            .mapToObj(i -> "num"+i)
            .collect(Collectors.toUnmodifiableList());
    list.add("not allowed");
}
```

## 小结

java10 最主要的新特性，在语法层面就属于 Local-Variable Type Inference，而在 jvm 方面 [307: Parallel Full GC for G1](http://openjdk.java.net/jeps/307) 以及 [317: Experimental Java-Based JIT Compiler](http://openjdk.java.net/jeps/317) 都比较重磅，值得深入研究。

## doc

- [JDK 10 Features](http://openjdk.java.net/projects/jdk/10/)
- [Introducing Java SE 10](https://blogs.oracle.com/java-platform-group/introducing-java-se-10)(`官方解读`)
- [What's New in JDK 10 - New Features and Enhancements](http://www.oracle.com/technetwork/java/javase/10-relnote-issues-4108729.html)(`官方细项解读`)
- [JDK-8140281 : (opt) add no-arg orElseThrow() as preferred alternative to get()](https://bugs.java.com/view_bug.do?bug_id=JDK-8140281)
- [JDK-8177290 : add copy factory methods for unmodifiable List, Set, Map](https://bugs.java.com/view_bug.do?bug_id=JDK-8177290)