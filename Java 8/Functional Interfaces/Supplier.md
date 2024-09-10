# Java 8 Supplier #

**Supplier** is a functional interface, it takes no arguments and returns a result a given generic type.


```java
@FunctionalInterface
public interface Supplier<T> {
      T get accept();
}
```

## 1. Supplier

This example uses **Supplier** to return a current date-time.

```java
package com.alejo.supplier1;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.function.Supplier;

public class SupplierExample1 {

    private static final DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    public static void main(String[] args) {

        Supplier<LocalDateTime> s = () -> LocalDateTime.now();
        LocalDateTime time = s.get();

        System.out.println(time);

        Supplier<String> s1 = () -> dtf.format(LocalDateTime.now());
        String time2 = s1.get();

        System.out.println(time2);
    }
}
```

## 2. Returns a Supplier


```java
package com.alejo.supplier1;

import java.util.ArrayList;
import java.util.List;
import java.util.function.Supplier;

public class SupplierExample2<T> {

    public static void main(String[] args) {
        Java8Supplier2<String> obj = new Java8Supplier2();
        List<String> list = obj.supplier().get();

    }

    public Supplier<List<T>> supplier() {

        // lambda
        // return () -> new ArrayList<>();

        // constructor reference
        return ArrayList::new;

    }
}
```


## 3. Factory

A simple factory method to return a **Developer** object.

```java
SupplierExample3.java

package com.alejo.supplier1;

import java.math.BigDecimal;
import java.time.LocalDate;
import java.util.function.Supplier;

public class SupplierExample3 {

    public static void main(String[] args) {

        Developer obj = factory(Developer::new);
        System.out.println(obj);

        Developer obj2 = factory(() -> new Developer("alejo"));
        System.out.println(obj2);

    }

    public static Developer factory(Supplier<? extends Developer> s) {

        Developer developer = s.get();
        if (developer.getName() == null || "".equals(developer.getName())) {
            developer.setName("default");
        }
        developer.setSalary(BigDecimal.ONE);
        developer.setStart(LocalDate.of(2017, 8, 8));

        return developer;

    }

}
```

```java
Developer.java

package com.alejo.supplier1;
import java.math.BigDecimal;
import java.time.LocalDate;

public class Developer {

    String name;
    BigDecimal salary;
    LocalDate start;

    // for factory(Developer::new);
    public Developer() {
    }

    // for factory(() -> new Developer("alejo"));
    public Developer(String name) {
        this.name = name;
    }

    // get, set, constructor, toString
    //...

}
```

```
Output:

Developer{name='default', salary=1, start=2017-08-08}
Developer{name='alejo', salary=1, start=2017-08-08}
```