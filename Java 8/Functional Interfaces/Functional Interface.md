# Functional Interface #

- Java 8 introduced **@FunctionalInterface**, an interface that has exactly one abstract method.

- The compiler will treat any interfaces meting the definition of a functional interface as a functional interface; it means the @FunctionalInterface annotation is optional.

- A functional interface can implement one or more methods by default, but you must necessarily have a single abstract method (Abstract method is a method without implementation).

- From Java 8 onwards, we can use lambda expressions to represent the instance of a functional interface.
- Some of the examples of functional interfaces in Java are Runnable, ActionListener, Comparable interfaces. We had to use anonymous inner class objects to implement functional interfaces before Java 8.

Six basics function interfaces:
  
|Interface|Signature|Example|
|---|---|---|
|UnaryOperator<T>|T apply(T t)|String::toLowerCase, Math::tan|
|BinaryOperator<T>| T apply(T t1, T t2)|BigInteger::add, Math::pow|
|Function<T, R>|  R apply(T t)| Arrays::asList, Integer::toBinaryString|
|Predicate<T, U>|boolean test(T t, U u)| String::isEmpty, Character::isDigit  |
|Supplier<T>|T get() | LocalDate::now, Instant::now|
|Consumer<T>|void accept(T t)|System.out::println, Error::printStackTrace|

It is recommended to use the `@FunctionalInterface` annotation to follow good development practices.

```java
public interface IFunctional {
  public String sayHelloTo(String namePerson);
} 
```

In this interface, we define a method that sends a greeting to the person that we pass as a parameter, but the body of the method is not yet defined.

```java
public interface IFunctional {
  
  public String sayHelloTo(String namePerson);
  
  public default String sayHelloWorld(){
    return "Hello World";
  }
}
```

This second interface is also a functional interface, since it only has an abstract method.

```java
@FunctionalInterface
public interface IFunctional {
  public String sayHelloTo(String namePerson);
  
  public default String sayHelloWorld(){
    return "Hello World";
  }
}
``` 

## Functional Interfaces with lambda expressions

Let's see how you could use the IStrategy class using lambda expressions:

```java
 public class Main{
  
   public static void main(){

      IFunctional functional1 = (name) -> "Hello " + name;
      System.out.println(functional1.sayHelloTo("Alejo Alvarez"));
      System.out.println(functional1.sayHelloWorld());
   }
 }  
  
@FunctionalInterface
interface IFunctional {
  public String sayHelloTo(String namePerson);
  
  public default String sayHelloWorld(){
    return "Hello World";
  }
}
``` 
  
  
Let's see how we define a lambda expression where "Hello" concatenates with the name of the person that is passed as a parameter, then this expression is assigned to the variable to the **functional1** variable, which is of type **IFunctional**. After this we call the sayHelloTo method which returns the greeting to "Alejo Alvarez", followed we call the sayHelloWord method, which sends a Hello Word.

```
Output:

Hello Alejo Alvarez
Hello world!
```

### Example Lambda using functional interface
```java
public class Example {
  public static void main(String args[]) {
    //lambda expression to create the object
    new Thread(() -> {
      System.out.println("Created a new thread");
    }).start();
  }
}
```