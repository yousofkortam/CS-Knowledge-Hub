# Java 8 BinaryOperator

**BinaryOperator** is a functional interface and it extends **BiFunction**.

The **BinaryOperator** takes two arguments of the same type and returns a result of the same type of its arguments.

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {

}
```

The **BiFunction** takes two argument of any type, and returns a result of any type.

```java
@FunctionalInterface
public interface BiFunction<T, U, R>{
    R apply(T t, U u);
}
```

## 1. BinaryOperator

In this example, the **BiFunction<Integer,Integer, Integer>** which accepts and return the same type, can be replaced with BinaryOperator `<Integer>`. 

```java
package com.alejo.binaryOperator1;

import java.util.function.BiFunction;
import java.util.function.BinaryOperator;

public class BinaryOperator1 {

    public static void main(String[] args) {

        // BiFunction
        BiFunction<Integer, Integer, Integer> func = (x1, x2) -> x1 + x2;

        Integer result = func.apply(2, 3);

        System.out.println(result); // 5

        // BinaryOperator
        BinaryOperator<Integer> func2 = (x1, x2) -> x1 + x2;

        Integer result2 = func.apply(2, 3);

        System.out.println(result2); // 5
    }
}

```

## 2. BinaryOperator as argument

This example simulates a **stream.reduce()** to sum all the **Integer**.

```java
package com.alejo.binaryOperator1;

import java.util.Arrays;
import java.util.List;
import java.util.function.BinaryOperator;

public class BinaryOperator2 {

    public static void main(String[] args) {

        Integer[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        Integer result = math(Arrays.asList(numbers), 0, (a, b) -> a + b);

        System.out.println(result); // 55

        Integer result2 = math(Arrays.asList(numbers), 0, Integer::sum);

        System.out.println(result2); // 55

    }

    public static <T> T math(List<T> list, T init, BinaryOperator<T> accumulator) {
        T result = init;
        for (T t : list) {
            result = accumulator.apply(result, t);
        }
        return result;
    }

}

```

## 3. IntBinaryOperator

If the math operations involve primitive types like **int**, change to **IntBinaryOperator** for better performance.

```java
package com.alejo.binaryOperator1;

import java.util.function.IntBinaryOperator;
import java.util.stream.IntStream;

public class BinaryOperator3 {

    public static void main(String[] args) {

        int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

        int result = math((numbers), 0, (a, b) -> a + b);

        System.out.println(result); // 55

        int result2 = math((numbers), 0, Integer::sum);

        System.out.println(result2); // 55 
    }

    public static int math(int[] list, int init, IntBinaryOperator accumulator) {
        int result = init;
        for (int t : list) {
            result = accumulator.applyAsInt(result, t);
        }
        return result;
    }

}
```

## 4. BinaryOperator.maxBy() and BinaryOperator.minBy()

This example uses **BinaryOperator** and a custom **Comparator** to find the highest and lowest pay developer from a list of developers.

```java
package com.alejo.binaryOperator1;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.BinaryOperator;

public class BinaryOperator4 {

    public static void main(String[] args) {

        Developer dev1 = new Developer("jordan", BigDecimal.valueOf(9999));
        Developer dev2 = new Developer("jack", BigDecimal.valueOf(8888));
        Developer dev3 = new Developer("alejo", BigDecimal.valueOf(10000));
        Developer dev4 = new Developer("ali", BigDecimal.valueOf(2000));
        Developer dev5 = new Developer("juan", BigDecimal.valueOf(1));

        List<Developer> list = Arrays.asList(dev1, dev2, dev3, dev4, dev5);

        // 1. Create a Comparator
        Comparator<Developer> comparing = Comparator.comparing(Developer::getSalary);

        // 2. BinaryOperator with a custom Comparator
        BinaryOperator<Developer> bo = BinaryOperator.maxBy(comparing);

        Developer result = find(list, bo);

        System.out.println(result);     // Developer{name='alejo', salary=10000}

        // one line

        // find developer with highest pay
        Developer developer = find(list, BinaryOperator.maxBy(Comparator.comparing(Developer::getSalary)));
        System.out.println(developer);  // Developer{name='alejo', salary=10000}

        // find developer with lowest pay
        Developer developer2 = find(list, BinaryOperator.minBy(Comparator.comparing(Developer::getSalary)));
        System.out.println(developer2); // Developer{name='juan', salary=1}

    }

    public static Developer find(List<Developer> list, BinaryOperator<Developer> accumulator) {
        Developer result = null;
        for (Developer t : list) {
            if (result == null) {
                result = t;
            } else {
                result = accumulator.apply(result, t);
            }
        }
        return result;
    }
}
```

```java
package com.alejo.binaryOperator1;

import java.math.BigDecimal;

public class Developer {

    String name;
    BigDecimal salary;

    public Developer(String name, BigDecimal salary) {
        this.name = name;
        this.salary = salary;
    }

    //...
}
```

```
Output:

Developer{name='jaden', salary=10000}
Developer{name='jaden', salary=10000}
Developer{name='mkyong', salary=1}
```