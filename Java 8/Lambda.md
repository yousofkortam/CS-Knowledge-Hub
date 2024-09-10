# Java 8 Lambda Expressions and Method References

<p align="center">
  <img src="https://user-images.githubusercontent.com/13514156/120511460-0f0ab900-c390-11eb-90af-6d0d827267a9.png">
</p>

Java 8 introduced lambda expressions to provide the implementation of the abstract method of a functional interface

Review th JDK **Iterable** class, it has a **default** method **forEach()** to accept a function interface **Consumer** 

```java
public interface Iterable<T> {

    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }

   //...
}
```

First, we can provide an anonymous class as the `forEach` implementation.

```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");

list.forEach(new Consumer<String>() { // anonymous class
    @Override
    public void accept(String str) {
        System.out.println(str);
    }
});
```

Alternatively, we can use a lambda expression to shorten the code like this:
```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(str -> System.out.println(str)); // lambda expressions
```

To gain better readability, we can replace lambda expression with method reference.

```java
List<String> list = Arrays.asList("node", "java", "python", "ruby");
list.forEach(System.out::println); // method references
```

## Lambda Scopes
Accessing outer scope variables from lambda expressions is very similar to anonymous objects. You can access final variables from the local outer scope as well as instance fields and static variables.

### Accessing local variables

We can read final variables from the outer scope of lambda expressions:

```java
final int num = 1;
Converter<Integer, String> stringConverter = (from) -> String.valueOf(from + num);
stringConverter.convert(2); // 3
```

But different to anonymous objects the variable num does not have to be declared final. This code is also valid:

```java
int num = 1;
Converter<Integer, String> stringConverter = (from) -> String.valueOf(from + num);
stringConverter.convert(2); // 3
```

However ```num``` must be implicitly final for the code to compile. The following code **does not** compile:

```java
int num = 1;
Converter<Integer, String> stringConverter = (from) -> String.valueOf(from + num);
num = 3;
```

Writing to num from within the lambda expression is also prohibited.

### Accessing fields and static variables

In contrast to local variables we have both read and write access to instance fields and static variables from within lambda expressions. This behavior is well known from anonymous objects.

```java
class LambdaExample {

    static int outerStaticNum;
    int outerNum;

    void testScopes() {
        Converter<Integer, String> stringConverter1 = (from) -> {
            outerNum = 23;
            return String.valueOf(from);
        };

        Converter<Integer, String> stringConverter2 = (from) -> {
            outerStaticNum = 72;
            return String.valueOf(from);
        };
    }
}
```

### Accessing Default Interface Methods

Remember the formula example from the first section? Interface `Formula` defines a default method `sqrt` which can be accessed from each formula instance including anonymous objects. This does not work with lambda expressions.

Default methods **cannot** be accessed from within lambda expressions. The following code does not compile:

```java
Formula formula = (a) -> sqrt( a * 100);
```

## Example 1

Let's start with a simple example of how to sort a list of strings in prior versions of Java:

```java
List<String> names = Arrays.asList("Alejo", "Juan", "Ana", "Maria");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
```

The static utility method Collections.sort accepts a list and a comparator in order to sort the elements of the given list. You often find yourself creating anonymous comparators and pass them to the sort method.

Instead of creating anonymous objects all day long, Java 8 comes with a much shorter syntax, lambda expressions:

```java
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```

As you can see the code is much shorter and easier to read. But it gets even shorter:

```java
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```

For one line method bodies you can skip both the braces {} and the return keyword. But it gets even shorter:

```java
names.sort((a, b) -> b.compareTo(a));
```

List now has a sort method. Also the java compiler is aware of the parameter types so you can skip them as well. Let's dive deeper into how lambda expressions can be used in the wild.

## Example 2

In this example, we will show you how to use java 8 Lambda expression to write a **Comparator** to sort a list.

Classic **Comparator** example.

```java
 Comparator<Developer> byName = new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getName().compareTo(o2.getName());
        }
};
```

Lambda expression equivalent.

```java
Comparator<Developer> byName = (Developer o1, Developer o2) -> o1.getName().compareTo(o2.getName());
```

## Example 3 
Sort without lambda

Example to compare the **Developer** objects using their age. Normally, you use **Collections.sort** and pass an anonymous **Comparator** class like this:

```java
package com.alejo.lambda1;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class TestSorting {

    public static void main(String[] args) {

        List<Developer> listDevs = getDevelopers();

        System.out.println("Before Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }
        
        //sort by age
        Collections.sort(listDevs, new Comparator<Developer>() {
            @Override
            public int compare(Developer o1, Developer o2) {
                return o1.getAge() - o2.getAge();
            }
        });
    
        System.out.println("After Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }
    }

    private static List<Developer> getDevelopers() {

        List<Developer> result = new ArrayList<Developer>();

        result.add(new Developer("alejo", new BigDecimal("70000"), 33));
        result.add(new Developer("maria", new BigDecimal("80000"), 20));
        result.add(new Developer("juan", new BigDecimal("100000"), 10));
        result.add(new Developer("laura", new BigDecimal("170000"), 55));
        
        return result;
    }
}
```

```
Output:

Before Sort
Developer [name=alejo, salary=70000, age=33]
Developer [name=maria, salary=80000, age=20]
Developer [name=juan, salary=100000, age=10]
Developer [name=laura, salary=170000, age=55]

After Sort
Developer [name=juan, salary=100000, age=10]
Developer [name=maria, salary=80000, age=20]
Developer [name=alejo, salary=70000, age=33]
Developer [name=laura, salary=170000, age=55]
```

When the sorting requirement is changed, you just pass in another new anonymos **Comparator** class:

```java
//sort by age
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getAge() - o2.getAge();
        }
    });
    
    //sort by name	
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getName().compareTo(o2.getName());
        }
    });
                
    //sort by salary
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getSalary().compareTo(o2.getSalary());
        }
    });
```

It works, but, do you think it is a bit weird to create a class just because you want to change a single line of code? 

## 2 Sort with Lambda

In Java 8, the **List** interface is supports the **sort** method directly, no need to use **Collection.sort** anymore.

```java
//List.sort() since Java 8
listDevs.sort(new Comparator<Developer>() {
    @Override
    public int compare(Developer o1, Developer o2) {
        return o2.getAge() - o1.getAge();
    }
});

```

Lambda expression example:

```java
package com.alejo.lambda1;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class TestSorting {

    public static void main(String[] args) {

        List<Developer> listDevs = getDevelopers();
        
        System.out.println("Before Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }
        
        System.out.println("After Sort");
        
        //lambda here!
        listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());
    
        //java 8 only, lambda also, to print the List
        listDevs.forEach((developer)->System.out.println(developer));
    }

    private static List<Developer> getDevelopers() {

        List<Developer> result = new ArrayList<Developer>();

        result.add(new Developer("alejo", new BigDecimal("70000"), 33));
        result.add(new Developer("maria", new BigDecimal("80000"), 20));
        result.add(new Developer("juan", new BigDecimal("100000"), 10));
        result.add(new Developer("laura", new BigDecimal("170000"), 55));
        
        return result;

    }
    
}
```

```
Output:

Before Sort
Developer [name=alejo, salary=70000, age=33]
Developer [name=maria, salary=80000, age=20]
Developer [name=juan, salary=100000, age=10]
Developer [name=laura, salary=170000, age=55]

After Sort
Developer [name=juan, salary=100000, age=10]
Developer [name=maria, salary=80000, age=20]
Developer [name=alejo, salary=70000, age=33]
Developer [name=laura, salary=170000, age=55]
```

## Examples Lambda Expressions

| Expressions | Descriptions |
|---|---|
| () -> {}  | No parameters; void result |
| () -> 91 | No parameters, expression body|
| () -> null | No parameters, expression body |
| () -> { return 91; } | No parameters, block body with return |
| () -> { System.gc(); } | No parameters, void block body |
| (int x) -> x+1 | Single declared-type argument|
| (int x) -> { return x+1; } | same as above|
| (x) -> x+1 | Single inferred-type argument, same as below|
| x -> x+1 | Parentheses optional for single inferred-type case|
| (String s) -> s.length() | Single declared-type argument|
| (Thread t) -> { t.start(); } | Single declared-type argument|
| s -> s.length() | Single inferred-type argument|
| t -> { t.start(); } | Single inferred-type argument|
| (int x, int y) -> x+y | Multiple declared-type parameters|
| (x,y) -> x+y | Multiple inferred-type parameters|
| (x, final y) -> x+y | Illegal: can’t modify inferred-type parameters|
| (x, int y) -> x+y | Illegal: can’t mix inferred and declared types|