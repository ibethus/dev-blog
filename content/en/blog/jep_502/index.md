---
title: 'JEP 502: Initialize Immutable Objects with StableValue'
date: 2025-04-11T18:20:06+02:00
draft: false
cover:
  image: cover.png
---

# Introduction

`StableValue` is a new feature introduced in the preview version of JDK 25, aimed at improving the handling of immutable objects in Java. This innovation allows certain dynamic values to be treated as constants, offering performance optimizations similar to `final` fields, while providing greater flexibility regarding when they are initialized.

The JEP related to this evolution is [JEP-502](https://openjdk.org/jeps/502), with the following defined objectives:

- Improve Java application startup time by **fragmenting the monolithic initialization** of application state.
- Decouple the **creation** of stable values from their **initialization**, without significant performance penalties.
- Ensure stable values are initialized **at most once**, even in **multi-threaded** programs.
- Allow user code to safely benefit from **constant folding**[^1] optimizations, just like variables declared as `final`.

## What is a Stable Value?

```java
public sealed interface StableValue<T>
```

A `StableValue` is an object encapsulating immutable data. Unlike `final` fields, which must be initialized upon object or class creation, Stable Values can be initialized on demand, after their declaration. This means they are only defined when their value is needed, which can improve startup performance of Java applications by avoiding loading the application's complete state at startup.

## Usage Examples

> All following examples are taken from the official JavaDoc[^2].

Here's a simple example of using `StableValue` in an `OrderController` class.

### Classic Version:

```java
class OrderController {

    private final Logger logger = Logger.create(OrderController.class);

    void submitOrder(User user, List<Product> products) {
        logger.info("order started");
        //...
        logger.info("order submitted");
    }

}
```
In this version, the logger is automatically instantiated each time an instance of the `OrderController` class is created. This could result in slower instantiation of this class, since we can imagine that creating this logger might be relatively slow (if it requires reading a config file for example, or preparing an output file for logs...).

### StableValue Version:

```java
class OrderController {

    private final StableValue<Logger> logger = StableValue.of();

    Logger getLogger() {
        return logger.orElseSet(() -> Logger.create(OrderController.class));
    }

    void submitOrder(User user, List<Product> products) {
        getLogger().info("order started");
        // ...
        getLogger().info("order submitted");
    }
}
```

In this example, the `logger` field is a `StableValue`. It is initialized only when the `getLogger()` method is called for the first time. This allows delaying the costly initialization of the logger until it is actually needed.

## Stable Function
`StableValue` lays the foundation for higher-level abstractions. For example, a `supplier` allows producing a value and caching its result:

```java
 public class Component {

     private final Supplier<Logger> logger =
             StableValue.supplier( () -> Logger.getLogger(Component.class) );

     public void process() {
        logger.get().info("Process started");
        // ...
     }
 }
```
Note that in this case, there is no accessor to the logger compared to the previous example, which makes its use more elegant.

## Stable Collection
Similarly, it is possible to create stable `Collection`s, whose values are calculated on first access:

```java
final class PowerOf2Util {

    private PowerOf2Util() {}

    private static final int SIZE = 6;
    private static final IntFunction<Integer> ORIGINAL_POWER_OF_TWO =
            v -> 1 << v;

    private static final List<Integer> POWER_OF_TWO =
            StableValue.list(SIZE, ORIGINAL_POWER_OF_TWO);

    public static int powerOfTwo(int a) {
        return POWER_OF_TWO.get(a);
    }
}

int pwr4 = PowerOf2Util.powerOfTwo(4);
```
> Note: The JVM will directly transform this value to 16 after a few execution cycles, thanks to constant folding.

## Composition
Finally, it is also possible to compose Stable Values, which allows for lazy evaluation of objects:

```java
 public final class DependencyUtil {

     private DependencyUtil() {}

     public static class Foo {
          // ...
      }

     public static class Bar {
         public Bar(Foo foo) {
              // ...
         }
     }

     private static final Supplier<Foo> FOO = StableValue.supplier(Foo::new);
     private static final Supplier<Bar> BAR = StableValue.supplier(() -> new Bar(FOO.get()));

     public static Foo foo() {
         return FOO.get();
     }

     public static Bar bar() {
         return BAR.get();
     }

 }
```

Calling `bar()` will create the `Bar` singleton if it doesn't already exist. During this creation, the dependent `Foo` will first be created if `Foo` does not already exist.

## Thread Safety

The content of a stable value is guaranteed to be defined at most once. If multiple threads compete to define a stable value, only one update succeeds (the first thread that manages to initialize the value), while other updates are blocked until the stable value is defined.

## Try It Yourself
To try this feature, download [jdk25](https://jdk.java.net/25/) then compile and run your program by enabling previews:

```bash
# Compilation
javac --release 25 --enable-preview Main.java
# Execution
java --enable-preview Main
```

## Conclusion

Stable Values represent a particularly interesting mechanism in the management of immutable objects in Java. By offering delayed initialization and performance optimizations, they allow developers to create more efficient and responsive applications.


[^1]: Constant folding [with javac](https://fekir.info/post/compile-time-optimizations-in-java/#_optimization-done-by-javac) and with [the JIT](https://jojozhuang.github.io/programming/java-advanced-jit-compiler/)
[^2]: https://cr.openjdk.org/~pminborg/stable-values2/api/java.base/java/lang/StableValue.html