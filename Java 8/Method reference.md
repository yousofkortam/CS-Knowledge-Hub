# Java 8 Method References #

The double colon (::) operator is called method references. Refer to the following examples:

They are a new feature of Java 8 that allows us to refer to the constructor methods through a functional interface, in other words, we can implement the functionality of an abstract method through the implementation of an already implemented method, assigning the implemented method to the abstract method. This can stand out a bit strange, especially because java was, until version 8, a too strict language.

Anonymous class to print a list.

```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(new Consumer<String>() {       // anonymous class
    @Override
    public void accept(String str) {
        System.out.println(str);
    }
});
```

Anonymous class with lambda expressions.

```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(str -> System.out.println(str)); // lambda
```

Lambda expression to Method references

```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(System.out::println);          // method references
```

There are four kinds of method references:

* Reference to a static method **ClassName::staticMethodName
* Reference to a instance method of a particular object **Object::instanceMethodName** 
* Reference to an instance method of an arbitrary object of a particular **ContainingType::methodName**
* Reference to a constructor **ClassName::new**

## 1. Reference to a static method

Lambda expression

```
(args) -> ClassName.staticMethodName(args)
```

Method reference
```
ClassName::staticMethodName
```

Example 1: This example prints a list of Strings, method reference to a static method **SimplePrinter::print**

```java
package com.alejo.methodreference1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Java8MethodReference1a {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("A", "B", "C");

        // anonymous class
        list.forEach(new Consumer<String>() {
            @Override
            public void accept(String x) {
                SimplePrinter.print(x);
            }
        });

        // lambda expression
        list.forEach(x -> SimplePrinter.print(x));

        // method reference
        list.forEach(SimplePrinter::print);
    }
}

class SimplePrinter {
    public static void print(String str) {
        System.out.println(str);
    }
}
```

Example 2: This example converts a list of String into a list of Interfers, method reference to a static method **Integer::parseInt**

```java
public static int parseInt(String s) throws NumberFormatException {
        return parseInt(s,10);
  }
```

```java
package com.alejo.methodreference1;

import java.util.Arrays;
import java.util.List;
import java.util.function.Function;
import java.util.stream.Collectors;

public class MethodReference1 {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("1", "2", "3");

        // anonymous class
        List<Integer> collect1 = list.stream()
                .map(new Function<String, Integer>() {
                    @Override
                    public Integer apply(String s) {
                        return Integer.parseInt(s);
                    }
                })
                .collect(Collectors.toList());

        // lambda expression
        List<Integer> collect2 = list.stream()
                .map(s -> Integer.parseInt(s))
                .collect(Collectors.toList());

        // method reference
        List<Integer> collect3 = list.stream()
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

```

Example 3: This example joins two **Integer** and returns a **String**. it passes a method reference static method **IntegerUtils::join** as an argument into another method that accepts a **BiFunction**.

```java
package com.alejo.methodreference1;

import java.util.function.BiFunction;

public class MethodReference1 {

    public static void main(String[] args) {

        // anonymous class
        String result1 = playTwoArgument(1, 2, new BiFunction<Integer, Integer, String>() {
                  @Override
                  public String apply(Integer a, Integer b) {
                      return IntegerUtils.join(a, b);
                  }
              });                                                                   // 3

        // lambda
        String result1 = playTwoArgument(1, 2, (a, b) -> IntegerUtils.join(a, b));  // 3

        // method reference
        String result2 = playTwoArgument(1, 2, IntegerUtils::join);                 // 3

    }

    private static <R> R playTwoArgument(Integer i1, Integer i2,
        BiFunction<Integer, Integer, R> func) {
        return func.apply(i1, i2);
    }

}

class IntegerUtils{

    public static String join(Integer a, Integer b) {
        return String.valueOf(a + b);
    }

}
```

## 2. Reference to an instance method of a particular object.

Lambda expression
```
(args) -> object.instanceMethodName(args)
```

Method reference
```
object::instanceMethodName
```

Example 1: This example sorts a list of **Employee** by salary. We can reference to an instance method **compareBySalary** of a particular object **ComparatorProvider** 

```java
package com.alejo.methodreference1;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.List;

public class MethodReference2 {

    public static void main(String[] args) {

        List<Employee> list = Arrays.asList(
                new Employee("alejo", 38, BigDecimal.valueOf(3800)),
                new Employee("maria", 5, BigDecimal.valueOf(100)),
                new Employee("juan", 25, BigDecimal.valueOf(2500)),
                new Employee("unknown", 99, BigDecimal.valueOf(9999)));

        // anonymous class
        /*list.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                return provider.compareBySalary(o1, o2);
            }
        });*/

        ComparatorProvider provider = new ComparatorProvider();

        // lambda
        // list.sort((o1, o2) -> provider.compareBySalary(o1, o2));

        // method reference
        list.sort(provider::compareBySalary);

        list.forEach(x -> System.out.println(x));
    }
}

class ComparatorProvider {

    public int compareByAge(Employee o1, Employee o2) {
        return o1.getAge().compareTo(o2.getAge());
    }

    public int compareByName(Employee o1, Employee o2) {
        return o1.getName().compareTo(o2.getName());
    }

    public int compareBySalary(Employee o1, Employee o2) {
        return o1.getAge().compareTo(o2.getAge());
    }

}
```

```java
package com.alejo.methodreference1;

import java.math.BigDecimal;

public class Employee {

    String name;
    Integer age;
    BigDecimal salary;

    // generated by IDE, getters, setters, constructor, toString
}
```

```
Output:

Employee{name='maria', age=5, salary=100}
Employee{name='juan', age=25, salary=2500}
Employee{name='alejo', age=38, salary=3800}
Employee{name='unknown', age=99, salary=9999}
```

## 3. Reference to an instance method of an arbitrary object of a particular type.

The statement is a bit confusing, need little explanation, see below samples:

Lambda expression.

```
// arg0 is the first argument
(arg0, rest_of_args) -> arg0.methodName(rest_of_args)

// example, assume a and b are String
(a, b) -> a.compareToIgnoreCase(b)
```

Method reference

```
// first argument type
arg0_Type::methodName

// arg0 is type of ClassName
ClassName::methodName

// example, a is type of String
String::compareToIgnoreCase
```

### Example 1

For **(String a, String b)**, where **a** and **b** are arbitrary names, and **String** is its arbitrary type. This examples uses method reference to an instance method **compareToIgnoreCase**  

```java
 String[] stringArray = { "Barbara", "James", "Mary", "John",
                "Patricia", "Robert", "Michael", "Linda" };
  Arrays.sort(stringArray, String::compareToIgnoreCase);
```

We passed a method reference **String::compareToIgnoreCase** as a comparator for **Array.sort** 

**Explanation**

Review the **Array.sort** method signature:

```java
public static <T> void sort (T[] a, Comparator<? super T> c){
}
```

In the above example, **Array.sort** expects a **Comparator```<String>```**. The **Comparator** is a function interface, its abstract method **compare** matches **BiFunction<String, String, Integer>**, it takes two arguments of **String** and return an **int**.

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);  // this matches BiFunction<String, String, Integer>
}
```

Review the **BiFunction** method signature:

```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
      R apply(T t, U u);
}
```

The below lambda provides implementation for **BiFunction<String, String, Integer>**, so the **Arrays.sort** accepts the below lambda expression as valid syntax.

```java
(String a, String b) -> a.compareToIgnoreCase(b) // return int

// a is type of String
// method reference
String::compareToIgnoreCase
```

### Example 2

```java
package com.alejo.methodreference1;

import java.util.function.BiPredicate;
import java.util.function.Function;

public class MethodReference3 {

    public static void main(String[] args) {

        // lambda
        int result = playOneArgument("alejo", x -> x.length());   // 5

        // method reference
        int result2 = playOneArgument("alejo", String::length);   // 5

        // lambda
        Boolean result3 = playTwoArgument("alejo", "l", (a, b) -> a.contains(b)); // true

        // method reference
        Boolean result4 = playTwoArgument("alejo", "l", String::contains);        // true

        // lambda
        Boolean result5 = playTwoArgument("alejo", "1", (a, b) -> a.startsWith(b)); // false

        // method reference
        Boolean result6 = playTwoArgument("alejo", "y", String::startsWith);        // false

        System.out.println(result6);
    }

    static <R> R playOneArgument(String s1, Function<String, R> func) {
        return func.apply(s1);
    }

    static Boolean playTwoArgument(String s1, String s2, BiPredicate<String, String> func) {
        return func.test(s1, s2);
    }

}
```

### Example 3

```java 
package com.alejo.methodreference1;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.function.BiFunction;

public class MethodReference3 {

    public static void main(String[] args) {

        Invoice obj = new Invoice("A001", BigDecimal.valueOf(1.99), 3);

        InvoiceCalculator formula = new InvoiceCalculator();

        // lambda
        BigDecimal result = calculate(formula, obj, (f, o) -> f.normal(o));         // 5.97

        // method reference
        BigDecimal result2 = calculate(formula, obj, InvoiceCalculator::normal);    // 5.97

        // lambda
        BigDecimal result3 = calculate(formula, obj, (f, o) -> f.promotion(o));     // 5.37

        // method reference
        BigDecimal result4 = calculate(formula, obj, InvoiceCalculator::promotion); // 5.37

    }

    static BigDecimal calculate(InvoiceCalculator formula, Invoice s1,
                                BiFunction<InvoiceCalculator, Invoice, BigDecimal> func) {
        return func.apply(formula, s1);
    }

}

class InvoiceCalculator {

    public BigDecimal normal(Invoice obj) {
        return obj.getUnitPrice().multiply(BigDecimal.valueOf(obj.qty));
    }

    public BigDecimal promotion(Invoice obj) {
        return obj.getUnitPrice()
                .multiply(BigDecimal.valueOf(obj.qty))
                .multiply(BigDecimal.valueOf(0.9))
                .setScale(2, RoundingMode.HALF_UP);
    }
}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    // generated by IDE, setters, gettes, constructor, toString
}
```

The first argument is a type of **InvoiceCalculator**. So we can reference to an instance method (**normal or promotion**) of an arbitrary object (**f**) of a particular type **InvoiceCalculator**

```java
(f, o) -> f.normal(o))
(f, o) -> f.promotion(o))

InvoiceCalculator::normal
InvoiceCalculator::promotion
```

## 4. Reference to a constructor.

Lambda expression
```java
(args) -> new ClassName (args)
```

Method Reference.
```java
ClassName::new
```

### Example1 

Reference to a default constructor.

```java
package com.alejo.methodreference1;

import java.math.BigDecimal;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Supplier;

public class MethodReference4 {

    public static void main(String[] args) {

        // lambda
        Supplier<Map> obj1 = () -> new HashMap();   // default HashMap() constructor
        Map map1 = obj1.get();

        // method reference
        Supplier<Map> obj2 = HashMap::new;
        Map map2 = obj2.get();

        // lambda
        Supplier<Invoice> obj3 = () -> new Invoice(); // default Invoice() constructor
        Invoice invoice1 = obj3.get();

        // method reference
        Supplier<Invoice> obj4 = Invoice::new;
        Invoice invoice2 = obj4.get();
    }
}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    public Invoice() {
    }

    //... generated by IDE
}
```

### Example 2

Reference to a constructor which accepts an argument **Invoice(BigDecimal unitPrice)**

```java
package com.alejo.methodreference1;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class Java8MethodReference4b {

    public static void main(String[] args) {

        List<BigDecimal> list = Arrays.asList(
                BigDecimal.valueOf(9.99),
                BigDecimal.valueOf(2.99),
                BigDecimal.valueOf(8.99));

        // lambda
        // List<Invoice> invoices = fakeInvoice(list, (price) -> new Invoice(price));

        // method reference
        List<Invoice> invoices = fakeInvoice(list, Invoice::new);

        invoices.forEach(System.out::println);
    }

    static List<Invoice> fakeInvoice(List<BigDecimal> list, Function<BigDecimal, Invoice> func) {
        List<Invoice> result = new ArrayList<>();

        for (BigDecimal amount : list) {
            result.add(func.apply(amount));
        }
        return result;
    }

}

class Invoice {

    String no;
    BigDecimal unitPrice;
    Integer qty;

    public Invoice(BigDecimal unitPrice) {
        this.unitPrice = unitPrice;
    }

    //... generated by IDE
}
```

```
Output:

Invoice{no='null', unitPrice=9.99, qty=null}
Invoice{no='null', unitPrice=2.99, qty=null}
Invoice{no='null', unitPrice=8.99, qty=null}
```