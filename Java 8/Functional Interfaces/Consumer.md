# Java 8 Consumer #

**Consumer** is a functional interface, it takes an argument and returns nothing, represents operations to be performed on a single input argument.


```java
@FunctionalInterface
public interface Consumer<T> {
      void accept(T t);
}
```

## 1. Consumer

```java
package com.alejo.consumer1;

import java.util.function.Consumer;

public class ConsumerExample1 {

    public static void main(String[] args) {

        Consumer<String> print = x -> System.out.println(x);
        print.accept("java");   // java

    }
}
```

## 2. Higher Order Function

This example accepts **Consumer** as an argument, simulates a **forEach** to print each item from a list.

```java
package com.alejo.consumer1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample2 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

        // implementation of the Consumer's accept methods.
        Consumer<Integer> consumer = (Integer x) -> System.out.println(x);
        forEach(list, consumer);

        // or call this directly
        forEach(list, (Integer x) -> System.out.println(x));

    }

    static <T> void forEach(List<T> list, Consumer<T> consumer) {
        for (T t : list) {
            consumer.accept(t);
        }
    }

}
```

```
Output:

1
2
3
4
5
1
2
3
4
5
```

Same **forEach** method to accept **Consumer** as an argument; this time, we will print the length of the string.

```java
package com.alejo.consumer1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample2 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("a", "ab", "abc");
        forEach(list, (String x) -> System.out.println(x.length()));

    }

    static <T> void forEach(List<T> list, Consumer<T> consumer) {
        for (T t : list) {
            consumer.accept(t);
        }
    }
}
```

```
Output:

1
2
3
```

