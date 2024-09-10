# Java 8 Function #

**Function** is a functional interface; it takes an argument (Object of type T) and returns an object (object of type R). The argument and output can be a different type.

The Java Function interface or java.util.function.Function interface is one of the most important functional interfaces

Default methods can be used to chain multiple functions together (compose, andThen).

```java
@FunctionalInterface
public interface Function<T, R> {
      R apply(T t);
}
```

* T - Type of the input to the function.
* R - Type of the result of the function.

## 1. Function <T,R>

This example takes a  `<T>` **String** and returns the length of the string as `<R>` **Integer**

```java
package com.alejo.functions1;

import java.util.function.Function;

public class FunctionExample {
    
    public static void main(String[] args){
        Function<String, Integer> func = x -> x.length();
        Integer apply = func.apply("Alejo"); // 5 
        System.out.println(apply);
    }

}

```

## 2. Chain Function <T, R>

This example chains the **Function** with **andThen()**.

```java
package com.alejo.functions1;

import java.util.function.Function;

public class FunctionsExample {

    public static void main(String[] args){
        Function<String, Integer> func = x -> x.length();
        Function<Integer, Integer> func2 = x -> x * 2;
        Integer result = func.andThen(func2).apply("Alejo"); // 10
        System.out.println(result);
    }
}

```

## 3. List to Map

This example accepts **Function** as an argument, convert a **List** into a ** Map**

```java
package com.alejo.functions1;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.function.Function;

public class Functions1 {
    public static void main(String[] args){
        Functions1 obj1 = new Functions1();
        List<String> list1 = Arrays.asList("java", "python", "node" );
        // By lambda
        Map<String, Integer> map1 = obj1.convertListToMap(list1, x -> x.length());
        System.out.print(map1); // {node=4, python=6, java=4}

        // By method reference
        Map<String, Integer> map2 = obj1.convertListToMap(list1, obj1::getLength);
        System.out.println(map2); // {node=4, python=6, java=4}
    }

    public <T, R> Map<T, R> convertListToMap (List<T> list, Function<T,R> func){
        Map<T, R> result = new HashMap<>();
        for (T t : list){
            result.put(t, func.apply(t));
        }
        return result;
    }

    public Integer getLength(String str){
        return str.length();
    }
}
```

## 4. List to List

This example accepts **Function** as an argument, convert a **List** of String into another **List** of String, which was hashed with SHA256.

```java
package com.alejo.functions1;

import org.apache.commons.codec.digest.DigestUtils;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class Functions1 {

    public static void main(String[] args) {

        Functions1 obj = new Functions1();

        List<String> list = Arrays.asList("node", "c++", "java", "javascript");

        // lambda
        //List<String> result = obj.map(list, x -> obj.sha256(x));

        // method reference
        List<String> result = obj.map(list, obj::sha256);

        result.forEach(System.out::println);

    }

    public <T, R> List<R> map(List<T> list, Function<T, R> func) {

        List<R> result = new ArrayList<>();
        for (T t : list) {
            result.add(func.apply(t));
        }
        return result;

    }

    // sha256 a string
    public String sha256(String str) {
        return DigestUtils.sha256Hex(str);
    }

}
```
You need add the dependency **commons-codec** for DigestUtils.sha256Hex