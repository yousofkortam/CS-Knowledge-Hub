# Java 8 BiPredicate

**BiPredicate** is a functional interface; which accepts two arguments and returns a boolean, basically this **BiPredicate** is same with the **Predicate**, instead, it takes 2 arguments for the test.

```java
@FunctionalInterface
public interface BiPredicate<T, U> {
      boolean test(T t, U u);
}
```

## 1. BiPredicate Hello World.

If the String length matches the provided length?.

```java
package com.alejo.bipredicate1;

import java.util.function.BiPredicate;

public class BiPredicateExample1 {

    public static void main(String[] args) {

        BiPredicate<String, Integer> filter = (x, y) -> {
            return x.length() == y;
        };

        boolean result = filter.test("Alejo", 5);
        System.out.println(result);  // true

        boolean result2 = filter.test("java", 10);
        System.out.println(result2); // false
    }

}
```

## 2. BiPredicate as function argument.

This example uses **BiPredicate**  to filter bad domains by the domain name or threat score.

```java
package com.alejo.predicate1;

import java.util.Arrays;
import java.util.List;
import java.util.function.BiPredicate;
import java.util.stream.Collectors;

public class BiPredicateExample2 {

    public static void main(String[] args) {

        List<Domain> domains = Arrays.asList(new Domain("google.com", 1),
                new Domain("i-am-spammer.com", 10),
                new Domain("alejo.com", 0),
                new Domain("microsoft.com", 2));

        BiPredicate<String, Integer> bi = (domain, score) -> {
            return (domain.equalsIgnoreCase("google.com") || score == 0);
        };

        // if google or score == 0
        List<Domain> result = filterBadDomain(domains, bi);
        System.out.println(result); // google.com, alejo.com

        //  if score == 0
        List<Domain> result2 = filterBadDomain(domains, (domain, score) -> score == 0);
        System.out.println(result2); // alejo.com, microsoft.com

        // if start with i or score > 5
        List<Domain> result3 = filterBadDomain(domains, (domain, score) -> domain.startsWith("i") && score > 5);
        System.out.println(result3); // i-am-spammer.com

        // chaining with or
        List<Domain> result4 = filterBadDomain(domains, bi.or(
                (domain, x) -> domain.equalsIgnoreCase("microsoft.com"))
        );
        System.out.println(result4); // google.com, alejo.com, microsoft.com
    }

    public static <T extends Domain> List<T> filterBadDomain(
            List<T> list, BiPredicate<String, Integer> biPredicate) {

        return list.stream()
                .filter(x -> biPredicate.test(x.getName(), x.getScore()))
                .collect(Collectors.toList());

    }
}

class Domain {

    String name;
    Integer score;

    public Domain(String name, Integer score) {
        this.name = name;
        this.score = score;
    }
    // getters , setters , toString
}
```

```
Output:

[Domain{name='google.com', score=1}, Domain{name='alejo.com', score=0}]
[Domain{name='alejo.com', score=0}]
[Domain{name='i-am-spammer.com', score=10}]
[Domain{name='google.com', score=1}, Domain{name='alejo.com', score=0}, Domain{name='microsoft.com', score=2}]
```