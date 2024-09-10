# Java 8 UnaryOperator #

**UnaryOperator** is a functional interface and it extends **Function**.

The **UnaryOperator** takes one argument, and returns a result of the same type of its arguments

```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T,T> {

}
```

The **Function** takes one argument of any type and returns a result of any type.

```java
@FunctionalInterface
public interface Function<T, R>{
    R apply(T t);
}
```

## 1. UnaryOperator

In this example, the **Function<Integer,Integer>** which accepts and returns the same type, can be replaced with UnaryOperator `<Integer>`. 

```java
package com.alejo.unaryOperator1;

import java.util.function.Function;
import java.util.function.UnaryOperator;

public class UnaryOperator1 {

    public static void main(String[] args) {

        Function<Integer, Integer> func = x -> x * 2;
        Integer result = func.apply(2);
        System.out.println(result);         // 4
        UnaryOperator<Integer> func2 = x -> x * 2;
        Integer result2 = func2.apply(2);
        System.out.println(result2);        // 4
    }
}
```

## 2. UnaryOperator as argument

```java
package com.alejo.unaryOperator1;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.UnaryOperator;

public class UnaryOperator2 {

    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        List<Integer> result = math(list, x -> x * 2);
        System.out.println(result); // [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
    }

    public static <T> List<T> math(List<T> list, UnaryOperator<T> uo) {
        List<T> result = new ArrayList<>();
        for (T t : list) {
            result.add(uo.apply(t));
        }
        return result;
    }
}
```

## 3. Chain UnaryOperator

```java
package com.alejo.unaryOperator1;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.UnaryOperator;

public class unaryOperator3 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        List<Integer> result = math(list,
                x -> x * 2,
                x -> x + 1);

        System.out.println(result); // [3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
    }

    public static <T> List<T> math(List<T> list,
                                   UnaryOperator<T> uo, UnaryOperator<T> uo2) {
        List<T> result = new ArrayList<>();
        for (T t : list) {
            result.add(uo.andThen(uo2).apply(t));
        }
        return result;
    }

}
```