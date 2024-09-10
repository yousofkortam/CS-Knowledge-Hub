# Java 8 BiConsumer #

**Consumer** is a functional interface, it takes two argument and returns nothing.

```java
@FunctionalInterface
public interface BiConsumer<T, U> {
      void accept(T t, U u);
}
```

## 1. BiConsumer

```java
package com.alejo.biconsumer1;

import java.util.function.Consumer;

public class BiConsumerExample1 {

    public static void main(String[] args) {

      BiConsumer<Integer, Integer> addTwo = (x, y) -> System.out.println(x + y);
      addTwo.accept(1, 2);    // 3
    }
}
```

## 2. Higher Order Function

This example accepts **BiConsumer** as an argument, create a generic **addTwo** to join two objects.

```java
package com.alejo.biconsumer1;

import java.util.function.BiConsumer;

public class BiConsumerExample2 {

    public static void main(String[] args) {

        addTwo(1, 2, (x, y) -> System.out.println(x + y));          // 3
        addTwo("Node", ".js", (x, y) -> System.out.println(x + y)); // Node.js

    }

    static <T> void addTwo(T a1, T a2, BiConsumer<T, T> c) {
        c.accept(a1, a2);
    }

}
```

More **BiConsumer** examples

```java
package com.alejo.biconsumer1;

import java.util.function.BiConsumer;

public class JavaBiConsumer3 {

    public static void main(String[] args) {

        math(1, 1, (x, y) -> System.out.println(x + y));   // 2
        math(1, 1, (x, y) -> System.out.println(x - y));   // 0
        math(1, 1, (x, y) -> System.out.println(x * y));   // 1
        math(1, 1, (x, y) -> System.out.println(x / y));   // 1

    }

    static <Integer> void math(Integer a1, Integer a2, BiConsumer<Integer, Integer> c) {
        c.accept(a1, a2);
    }
}

```

## 3. Map.forEach

In the JDK source code, **Map.forEach** accepts a **BiConsumer**  as a argument


```java
default void forEach(BiConsumer<? super K, ? super V> action) {
        Objects.requireNonNull(action);
        for (Map.Entry<K, V> entry : entrySet()) {
            K k;
            V v;
            try {
                k = entry.getKey();
                v = entry.getValue();
            } catch (IllegalStateException ise) {
                // this usually means the entry is no longer in the map.
                throw new ConcurrentModificationException(ise);
            }
            action.accept(k, v);
        }
    }
```

```java
package com.alejo.biconsumer1;

public class BiConsumerExample3 {

    public static void main(String[] args) {

        Map<Integer, String> map = new LinkedHashMap<>();

        map.put(1, "Java");
        map.put(2, "C++");
        map.put(3, "Rust");
        map.put(4, "JavaScript");
        map.put(5, "Go");

        map.forEach((k, v) -> System.out.println(k + ":" + v));

    }

}
```