# Streams


- A java.util.Stream represents a sequence of elements on which one or more operations can be performed.
- It is a Java API for handling data in collections. 
- Streams are a sequence of elements that support sequential and parallel aggregation operations.
- One of the new Java 8 features that allows you to manipulate collections like never before, almost dreamlike.

Streams are `Monads` (thus playing a big part int bringing functional programming, a monads is a structure that represents computations defined as sequences of steps. A type with a monad structure defines what it means to chain operations, or nest functions of that type together).

Well, Streams do not allow creating shortcuts when processing collections, creating data flows that allow processing in a declarative way, that is, we focus on what we want to solve, and not on how we should do it, as it happens in imperative programming.

Let's see the simplest case when using a collection, let's print all the values ​​of a list Simple isn't it?

```java
public static void printForeach(){
  List<String> names = getStringArray();
  for (String name : names) {
     System.out.println(name);
  }
}
```

With Streams
```java
public static void printStream(){
    List<String> names = getStringArray();
    names.stream()
         .forEach(System.out::println);
}
```

Note that we are using the **stream()** method to obtain the Stream, followed by, we only use the **forEach()** method in which we define what we are going to do for each element of the collection. In this case we are printing each employee using **Method Reference.**

Imagine that streams are a way to iterate all the elements of the collection in a simple way, and that while you iterate them you can perform operations on the collection in a declarative way. That is, we are going to perform operations but focusing on the objective we seek, not on how to implement the algorithm to make it work. Let's look at another more complex example.

## Example

We have a list of Employees, each employee has a DNI that identifies him as unique, has a name and the department to which he belongs. The list of employees is as follows:

<p>

| Id | Name | Department |
|---|---|---|
| 1  | Alejo Alvarez   |  Systes |
| 2  | Maria Gutierrez  | Sales  |
| 3  | Manuela Rojas  |  RH |
| 4  | Pepito Perez  |  Sales |
| 5  | Laura Pulido |  Sales |
</p> </br>

Now, on this list we want to filter all the Employees that belong to the systems department, then we are going to order all the employees in ascending order by their name, then we are going to filter the repeated elements by their ID, finally, we will print the name of all employees who followed all the rules.

```java
public static void printOrderedSystemEmployees(){
    List<Employee> employees = getEmployeeArray();
    employees.stream()
            .filter(x -> x.getDepartment().equals("Systems"))
            .sorted((x,y) -> x.getName().compareToIgnoreCase(y.getName()))
            .distinct()
            .forEach(System.out::println);
}
```

## How streams work

A stream represents a sequence of elements and supports different kind of operations to perform computations upon those elements:


```java
List<String> myList = Arrays.asList("a1", "a2", "b1", "c2", "c1");

myList.stream()
      .filter(s -> s.startsWith("c"))
      .map(String::toUpperCase)
      .sorted()
      .forEach(System.out::println);
```

```
Output
C1
C2
```

Stream operations are either intermediate or terminal. Intermediate operations return a stream so we can chain multiple intermediate operations without using semicolons.

Terminal operations are either void or return a non-stream result. In the above example `filter`, `map` and `sorted` are intermediate operations whereas `forEach` is a terminal operation.

 Such a chain of stream operations as seen in the example above is also known as operation pipeline.

Most stream operations accept some kind of lambda expression parameter, a functional interface specifying the exact behavior of the operation. Most of those operations must be both non-interfering and stateless. What does that mean?

A function is [non-interfering](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#NonInterference) when it does not modify the underlying data source of the stream, e.g. in the above example no lambda expression does modify myList by adding or removing elements from the collection.

A function is [stateless](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#Statelessness) when the execution of the operation is deterministic, e.g. in the above example no lambda expression depends on any mutable variables or states from the outer scope which might change during execution.

### Processing Order
Now that we've learned how to create and work with different kinds of streams, let's dive deeper into how stream operations are processed under the hood.

An important characteristic of intermediate operations is laziness. Look at this sample where a terminal operation is missing:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return true;
    });
```

When executing this code snippet, nothing is printed to the console. That is because intermediate operations will only be executed when a terminal operation is present.

Let's extend the above example by the terminal operation `forEach`:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return true;
    })
    .forEach(s -> System.out.println("forEach: " + s));
```

Executing this code snippet results in the desired output on the console:

```
Output: 

filter:  d2
forEach: d2
filter:  a2
forEach: a2
filter:  b1
forEach: b1
filter:  b3
forEach: b3
filter:  c
forEach: c
```

The order of the result might be surprising. A naive approach would be to execute the operations horizontally one after another on all elements of the stream. But instead each element moves along the chain vertically. The first string "d2" passes `filter` then `forEach`, only then the second string "a2" is processed.

This behavior can reduce the actual number of operations performed on each element, as we see in the next example:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .map(s -> {
        System.out.println("map: " + s);
        return s.toUpperCase();
    })
    .anyMatch(s -> {
        System.out.println("anyMatch: " + s);
        return s.startsWith("A");
    });

```

```
Output

map: d2
anyMatch: D2
map: a2
anyMatch: A2
```

The operation `anyMatch` returns `true` as soon as the predicate applies to the given input element. This is true for the second element passed "A2". Due to the vertical execution of the stream chain, ```map``` has only to be executed twice in this case. So instead of mapping all elements of the stream, ```map``` will be called as few as possible.

### Why order matters
The next example consists of two intermediate operations `ma` and `filter` and the terminal operation `forEach`. Let's once again inspect how those operations are being executed:    
    
```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .map(s -> {
        System.out.println("map: " + s);
        return s.toUpperCase();
    })
    .filter(s -> {
        System.out.println("filter: " + s);
        return s.startsWith("A");
    })
    .forEach(s -> System.out.println("forEach: " + s));

```

```
Output

map: d2
filter: D2
map: a2
filter: A2
forEach: A2
map: b1
filter: B1
map: b3
filter: B3
map: c
filter: C
```

As you might have guessed both `map` and `filter` are called five times for every string in the underlying collection whereas `forEach` is only called once.

We can greatly reduce the actual number of executions if we change the order of the operations, moving `filter` to the beginning of the chain:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return s.startsWith("a");
    })
    .map(s -> {
        System.out.println("map: " + s);
        return s.toUpperCase();
    })
    .forEach(s -> System.out.println("forEach: " + s));

```

```
Output

filter:  d2
filter:  a2
map:     a2
forEach: A2
filter:  b1
filter:  b3
filter:  c
```

Now, `map` is only called once so the operation pipeline performs much faster for larger numbers of input elements. Keep that in mind when composing complex method chains.

Let's extend the above example by an additional operation, `sorted`:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .sorted((s1, s2) -> {
        System.out.printf("sort: %s; %s\n", s1, s2);
        return s1.compareTo(s2);
    })
    .filter(s -> {
        System.out.println("filter: " + s);
        return s.startsWith("a");
    })
    .map(s -> {
        System.out.println("map: " + s);
        return s.toUpperCase();
    })
    .forEach(s -> System.out.println("forEach: " + s));
    
    .forEach(s -> System.out.println("forEach: " + s));
  ```
  
Sorting is a special kind of intermediate operation. It's a so called stateful operation since in order to sort a collection of elements you have to maintain state during ordering.

Executing this example results in the following console output:

```
sort:    a2; d2
sort:    b1; a2
sort:    b1; d2
sort:    b1; a2
sort:    b3; b1
sort:    b3; d2
sort:    c; b3
sort:    c; d2
filter:  a2
map:     a2
forEach: A2
filter:  b1
filter:  b3
filter:  c
filter:  d2
```

First, the sort operation is executed on the entire input collection. In other words sorted is executed horizontally. So in this case ```sorted``` is called eight times for multiple combinations on every element in the input collection.

Once again we can optimize the performance by reordering the chain:

```java
Stream.of("d2", "a2", "b1", "b3", "c")
    .filter(s -> {
        System.out.println("filter: " + s);
        return s.startsWith("a");
    })
    .sorted((s1, s2) -> {
        System.out.printf("sort: %s; %s\n", s1, s2);
        return s1.compareTo(s2);
    })
    .map(s -> {
        System.out.println("map: " + s);
        return s.toUpperCase();
    })
    .forEach(s -> System.out.println("forEach: " + s));
```

```
Output

filter:  d2
filter:  a2
filter:  b1
filter:  b3
filter:  c
map:     a2
forEach: A2
```

In this example ```sorted``` is never been called because ```filter``` reduces the input collection to just one element. So the performance is greatly increased for larger input collections.