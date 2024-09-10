# Streams - filter()

<p align="center">
<img height="250" src="https://user-images.githubusercontent.com/13514156/120511063-a58aaa80-c38f-11eb-8f15-0adc5e3370c0.png">
</p>

## Example 1 - Streams filter() and collect()

In this example, `streams.filter()` to filter a `List`, and `collect()` to convert a streams into a `List`

```java
package com.alejo.streams1;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamsFilter1 {

    public static void main(String[] args) {

        List<String> lines = Arrays.asList("spring", "node", "java");

        List<String> result = lines.stream()   // convert list to stream
                .filter(line -> !"node".equals(line))  // omit node
                .collect(Collectors.toList());    // collect the output and convert streams to a List

        result.forEach(System.out::println);   // spring, java
    }
}
```

## Example 2 -Streams filter(), findAny() and orElse()

```java
package com.alejo.streams1;

public class Person {

    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    //getters, setters, toString
}
```

Before java 8, you get a **Person** by name like this:

```java
package com.alejo.streams1;

import java.util.Arrays;
import java.util.List;

public class StreamsFilter2 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("alejo", 30),
                new Person("juan", 20),
                new Person("ana", 40)
        );

        Person result = getStudentByName(persons, "juan");
        System.out.println(result);

    }

    private static Person getStudentByName(List<Person> persons, String name) {

        Person result = null;
        for (Person temp : persons) {
            if (name.equals(temp.getName())) {
                result = temp;
            }
        }
        return result;
    }
}
```

```
Output:

Person{name= 'juan', age=20}
```

The equivalent example in Java8, use **stream.filter()** to filter a **List**, and ,`findAny().orElse` (null) to return an object conditional

```java
package com.alejo.streams1;

import java.util.Arrays;
import java.util.List;

public class StreamsFilter2 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("alejo", 30),
                new Person("juan", 20),
                new Person("ana", 40)
        );

        Person result1 = persons.stream() // Convert to steam
                .filter(x -> "juan".equals(x.getName())) // we want "juan" only
                .findAny() // If 'findAny' then return found
                .orElse(null); // If not found, return null

        System.out.println(result1);
        
        Person result2 = persons.stream()
                .filter(x -> "test1".equals(x.getName()))
                .findAny()
                .orElse(null);

        System.out.println(result2);

    }
}
```

```
Output: 

Person{name='juan', age=20}
null
```

## Example 3 - Streams filter() and map()

```java
package com.alejo.streams1;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamsFilter3 {

    public static void main(String[] args) {

        List<Person> persons = Arrays.asList(
                new Person("alejo", 30),
                new Person("juan", 20),
                new Person("anae", 40)
        );

        String name = persons.stream()
                .filter(x -> "juan".equals(x.getName()))
                .map(Person::getName) //convert stream to String
                .findAny()
                .orElse("");

        System.out.println("name : " + name);

        List<String> collect = persons.stream()
                .map(Person::getName)
                .collect(Collectors.toList());

        collect.forEach(System.out::println);

    }
    
}
```

```
Output: 
name: juan

alejo
juan
ana
```