# Predicate

**Predicate** is a functional interface; which accepts an argument and returns a boolean. Usually, it used to apply in a filter for a collection of objects.

The interface contains several default methods for composing predicates to complex logical terms (and, or, negate)

```java
@FunctionalInterface
public interface Predicate<T> {

      boolean test(T t);

}
```

## 1. Predicate in filter()

**filter()** accepts predicate as argument.


```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class PredicateExample1 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream().filter(x -> x > 5).collect(Collectors.toList());

        System.out.println(collect); // [6, 7, 8, 9, 10]
    }
}
```

```java

package com.alejo.functions1;

public class FunctionsExample {

    public static void main(String[] args) {

        Predicate<Integer> noGreaterThan5 =  x -> x > 5;

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream()
                .filter(noGreaterThan5)
                .collect(Collectors.toList());

        System.out.println(collect); // [6, 7, 8, 9, 10]

    }

}
```

## 2. Predicate.and()

Multiple filters.

```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class PredicateExample2 {

    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // multiple filters
        List<Integer> collect = list.stream()
                .filter(x -> x > 5 && x < 8).collect(Collectors.toList());

        System.out.println(collect);

    }

}
```


```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample2 {

    public static void main(String[] args) {

        Predicate<Integer> noGreaterThan5 = x -> x > 5;
        Predicate<Integer> noLessThan8 = x -> x < 8;

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> collect = list.stream()
                .filter(noGreaterThan5.and(noLessThan8))
                .collect(Collectors.toList());

        System.out.println(collect);

    }

}
```

## 3. Predicate.or()


```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample3 {

    public static void main(String[] args) {

        Predicate<String> lengthIs3 = x -> x.length() == 3;
        Predicate<String> startWithA = x -> x.startsWith("A");

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        List<String> collect = list.stream()
                .filter(lengthIs3.or(startWithA))
                .collect(Collectors.toList());

        System.out.println(collect); // [A, AA, AAA, BBB]
    }
}
```

## 4. Predicate.negative()

Find all elements not start with 'A'

```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample4 {

    public static void main(String[] args) {

        Predicate<String> startWithA = x -> x.startsWith("A");

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        List<String> collect = list.stream()
                .filter(startWithA.negate())
                .collect(Collectors.toList());

        System.out.println(collect); // [B, BB, BB]

    }
}
```

## 5. Predicate.test() in function

Predicate in function

```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample5 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("A", "AA", "AAA", "B", "BB", "BBB");

        System.out.println(StringProcessor.filter(
                list, x -> x.startsWith("A")));                    // [A, AA, AAA]

        System.out.println(StringProcessor.filter(
                list, x -> x.startsWith("A") && x.length() == 3)); // [AAA]

    }

}

class StringProcessor {
    static List<String> filter(List<String> list, Predicate<String> predicate) {
        return list.stream().filter(predicate::test).collect(Collectors.toList());
    }
}
```

## 6. Predicate Chaining

We can chain predicates together.

```java
package com.alejo.predicate1;

import java.util.function.Predicate;

public class PredicateExample6 {

    public static void main(String[] args) {

        Predicate<String> startWithA = x -> x.startsWith("a");

        // start with "a" or "m"
        boolean result = startWithA.or(x -> x.startsWith("m")).test("mkyong");
        System.out.println(result);     // true

        // !(start with "a" and length is 3)
        boolean result2 = startWithA.and(x -> x.length() == 3).negate().test("abc");
        System.out.println(result2);    // false

    }

}
```

## 7. Predicate in Object


```java
package com.alejo.predicate1;

public class Hosting {

    private int Id;
    private String name;
    private String url;

    public Hosting(int id, String name, String url) {
        Id = id;
        this.name = name;
        this.url = url;
    }

    //... getters and setters, toString()
}
```

```java
package com.alejo.predicate1;

import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class HostingRepository {

    public static List<Hosting> filterHosting(List<Hosting> hosting,
                                              Predicate<Hosting> predicate) {
        return hosting.stream()
                .filter(predicate)
                .collect(Collectors.toList());
    }

}
```
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class PredicateExample7 {

    public static void main(String[] args) {

        Hosting h1 = new Hosting(1, "amazon", "aws.amazon.com");
        Hosting h2 = new Hosting(2, "netflix", "netflix.com");
        Hosting h3 = new Hosting(3, "any", "any.com");
        Hosting h4 = new Hosting(4, "google", "google.com");

        List<Hosting> list = Arrays.asList(new Hosting[]{h1, h2, h3, h4});

        List<Hosting> result = HostingRepository.filterHosting(list, x -> x.getName().startsWith("g"));
        System.out.println("result : " + result);  // google

        List<Hosting> result2 = HostingRepository.filterHosting(list, isDeveloperFriendly());
        System.out.println("result2 : " + result2); // netflix

    }

    public static Predicate<Hosting> isDeveloperFriendly() {
        return n -> n.getName().equals("netflix");
    }
}
```

```
Output:
result : [Hosting{Id=4, name='google', url='google.com'}]
result2 : [Hosting{Id=2, name='netflix', url='netflix.com'}]

```