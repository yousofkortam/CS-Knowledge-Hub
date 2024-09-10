# Java 8 BiFunction #

**BiFunction** is a functional interface; it takes two arguments and returns an object.


```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
      R apply(T t, U u);
}
```

* T - Type of the first argument to the function.
* U - Type of the second argument to the function.
* R - Type of the result of the function.

## 1. BiFunction <T,U, R>

This example takes two **Integer** and returns an **Integer**, **Double** or **List**

```java
package com.alejo.bifunctions1;

import java.util.Arrays;
import java.util.List;
import java.util.function.BiFunction;

public class BiFunctionExample1 {

    public static void main(String[] args) {

        // takes two Integers and return an Integer
        BiFunction<Integer, Integer, Integer> func = (x1, x2) -> x1 + x2;

        Integer result = func.apply(2, 3);

        System.out.println(result); // 5

        // take two Integers and return an Double
        BiFunction<Integer, Integer, Double> func2 = (x1, x2) -> Math.pow(x1, x2);

        Double result2 = func2.apply(2, 4);

        System.out.println(result2);    // 16.0

        // take two Integers and return a List<Integer>
        BiFunction<Integer, Integer, List<Integer>> func3 = (x1, x2) -> Arrays.asList(x1 + x2);

        List<Integer> result3 = func3.apply(2, 3);

        System.out.println(result3); // [5]

    }

}

```

## 2. BiFunction <T,U,R> + Function <T,R>

This **BiFunction** takes two **Integer** and return a **Double**, and uses **andThen** to chain it with a **Function** to convert the **Double** into a **String**.

```java
package com.alejo.bifunctions1;

import java.util.function.BiFunction;
import java.util.function.Function;

public class FunctionsExample {

    public static void main(String[] args) {

        // Math.pow(a1, a2) returns Double
        BiFunction<Integer, Integer, Double> func1 = (a1, a2) -> Math.pow(a1, a2);

        // takes Double, returns String
        Function<Double, String> func2 = (input) -> "Result : " + String.valueOf(input);

        String result = func1.andThen(func2).apply(2, 4);
        System.out.println(result); // 16.0
    }
}
```

## 3. Factory

This example uses **BiFunction** to create an object, acts as a factory pattern.

```java
package com.alejo.bifunctions1;

import java.util.function.BiFunction;

public class BiFunctionsExample2 {

    public static void main(String[] args) {

        GPS obj = factory("39.741895", "-63.989308", GPS::new);
        System.out.println(obj); // GPS{Latitude='39.741895', Longitude='-63.989308'}

    }

    public static <R extends GPS> R factory(String Latitude, String Longitude,
                                            BiFunction<String, String, R> func) {
        return func.apply(Latitude, Longitude);
    }

}

class GPS {

    String Latitude;
    String Longitude;

    public GPS(String latitude, String longitude) {
        Latitude = latitude;
        Longitude = longitude;
    }

    public String getLatitude() {
        return Latitude;
    }

    public void setLatitude(String latitude) {
        Latitude = latitude;
    }

    public String getLongitude() {
        return Longitude;
    }

    public void setLongitude(String longitude) {
        Longitude = longitude;
    }

    @Override
    public String toString() {
        return "GPS{" +
                "Latitude='" + Latitude + '\'' +
                ", Longitude='" + Longitude + '\'' +
                '}';
    }
}
```

## 4. More

Filtering a **List** by some conditions.

```java
package com.alejo.functions1;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.BiFunction;

public class BiFunctionsExample3 {

    public static void main(String[] args) {

        BiFunctionsExample3 obj = new BiFunctionsExample3();

        List<String> list = Arrays.asList("node", "c++", "java", "javascript");

        List<String> result = obj.filterList(list, 3, obj::filterByLength);

        System.out.println(result);   // [node, java, javascript]

        List<String> result1 = obj.filterList(list, 3, (l1, size) -> {
            if (l1.length() > size) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result1);  // [node, java, javascript]

        List<String> result2 = obj.filterList(list, "c", (l1, condition) -> {
            if (l1.startsWith(condition)) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result2);  // [c++]

        List<Integer> number = Arrays.asList(1, 2, 3, 4, 5);

        List<Integer> result3 = obj.filterList(number, 2, (l1, condition) -> {
            if (l1 % condition == 0) {
                return l1;
            } else {
                return null;
            }
        });

        System.out.println(result3);  // [2, 4]

    }

    public String filterByLength(String str, Integer size) {
        if (str.length() > size) {
            return str;
        } else {
            return null;
        }
    }

    public <T, U, R> List<R> filterList(List<T> list1, U condition,
                                        BiFunction<T, U, R> func) {

        List<R> result = new ArrayList<>();

        for (T t : list1) {
            R apply = func.apply(t, condition);
            if (apply != null) {
                result.add(apply);
            }
        }

        return result;

    }

}
```