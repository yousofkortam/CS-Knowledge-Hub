# Java 8 - forEach() Examples

In Java 8, we can use the new `forEach` to loop or **iterate** a `Map`, `List`, `Set`, or `Stream`.

## 1 Loop a Map

This is a normal way to loop a **Map**

```java
public static void loopMapClassic() {

    Map<String, Integer> map = new HashMap<>();
    map.put("A", 10);
    map.put("B", 20);
    map.put("C", 30);
    map.put("D", 40);
    map.put("E", 50);
    map.put("F", 60);

    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        System.out.println("Key : " + entry.getKey() + ", Value : " + entry.getValue());
    }
}
```

The same but forEach

```java
public static void loopMapJava8() {

    Map<String, Integer> map = new HashMap<>();
    map.put("A", 10);
    map.put("B", 20);
    map.put("C", 30);
    map.put("D", 40);
    map.put("E", 50);
    map.put("F", 60);

    // lambda
    map.forEach((k, v) -> System.out.println("Key : " + k + ", Value : " + v));

}
```

```
Output:

Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : D, Value : 40
Key : E, Value : 50
Key : F, Value : 60
```

For the Map‘s key or value containing null, the forEach will print null.

```java

public static void loopMapJava8() {

    Map<String, Integer> map = new HashMap<>();
    map.put("A", 10);
    map.put("B", 20);
    map.put("C", 30);
    map.put(null, 40);
    map.put("E", null);
    map.put("F", 60);

    // ensure map is not null
    if (map != null) {
        map.forEach((k, v) -> System.out.println("Key : " + k + ", Value : " + v));
    }

}
```

```
Output:

Key : null, Value : 40
Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : E, Value : null
Key : F, Value : 60
```

If we do not want to print the null key, add a simple null checking inside the forEach.

```java
public static void loopMapJava8() {

    Map<String, Integer> map = new HashMap<>();
    map.put("A", 10);
    map.put("B", 20);
    map.put("C", 30);
    map.put(null, 40);
    map.put("E", null);
    map.put("F", 60);

    map.forEach(
        (k, v) -> {
            // yes, we can put logic here
            if (k != null){
                System.out.println("Key : " + k + ", Value : " + v);
            }
        }
    );
}
```

```
Output:

Key : A, Value : 10
Key : B, Value : 20
Key : C, Value : 30
Key : E, Value : null
Key : F, Value : 60
```

## 2. Loop a List

This is a normal way to loop a **List**

```java
public static void loopListClassic() {

    List<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("D");
    list.add("E");

    // normal loop
    for (String l : list) {
        System.out.println(l);
    }

}
```

Java 8 **forEach** to loop a **List**

```java
public static void loopListJava8() {

    List<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
    list.add("D");
    list.add("E");

    // lambda
    // list.forEach(x -> System.out.println(x));

    // method reference
    list.forEach(System.out::println);
}
```

```
Output:

A
B
C
D
E
```

This example filter the null value of a **List**

```java
 public static void loopListJava8() {

    List<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add(null);
    list.add("D");
    list.add("E");

    // filter null value
    list.stream()
            .filter(Objects::nonNull)
            .forEach(System.out::println);

}
``` 

```
Output:

A
B
D
E
```

The forEach for **Set** and **Streams** works the same way.

## 3. forEach and Consumer

Review the **forEach** method signature, it accepts a functional interface **Consumer**.


```java
Iterable.java

public interface Iterable<T> {

  default void forEach(Consumer<? super T> action) {
      Objects.requireNonNull(action);
      for (T t : this) {
          action.accept(t);
      }
  }
  //..
}
```

```java
public interface Stream<T> extends BaseStream<T, Stream<T>> {

  void forEach(Consumer<? super T> action);
  //...

}
```

This example creates a **Consumer** method to print String to its Hex format. We can now reuse the same **Consumer** method and pass it to the forEach method of **List** and **Stream**.

```java
package com.alejo.foreach;

import java.util.*;
import java.util.function.Consumer;
import java.util.stream.Stream;

public class ForEachConsumer {

    public static void main(String[] args) {

        List<String> list = Arrays.asList("abc", "java", "python");
        Stream<String> stream = Stream.of("abc", "java", "python");

        // convert a String to a Hex
        Consumer<String> printTextInHexConsumer = (String x) -> {
            StringBuilder sb = new StringBuilder();
            for (char c : x.toCharArray()) {
                String hex = Integer.toHexString(c);
                sb.append(hex);
            }
            System.out.print(String.format("%n%-10s:%s", x, sb.toString()));
        };

        // pass a Consumer
        list.forEach(printTextInHexConsumer);

        stream.forEach(printTextInHexConsumer);

    }
}
```

```
Output:

abc       :616263
java      :6a617661
python    :707974686f6e

abc       :616263
java      :6a617661
python    :707974686f6e
```

## 4. forEach and Exception handling

The **forEach** is not just for printing, and this example shows how to use **forEach** method to loop a list of objects and write it to files.

```java
package com.alejo.foreach;

import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.List;

public class ForEachWriteFile {

    public static void main(String[] args) {

        ForEachWriteFile obj = new ForEachWriteFile();
        obj.save(Paths.get("C:\\test"), obj.createDummyFiles());
    }

    public void save(Path path, List<DummyFile> files) {

        if (!Files.isDirectory(path)) {
            throw new IllegalArgumentException("Path must be a directory");
        }

        files.forEach(f -> {
            try {
                int id = f.getId();
                // create a filename
                String fileName = id + ".txt";
                Files.write(path.resolve(fileName),
                        f.getContent().getBytes(StandardCharsets.UTF_8));
            } catch (IOException e) {
                e.printStackTrace();
            }
        });

    }

    public List<DummyFile> createDummyFiles() {
        return Arrays.asList(
                new DummyFile(1, "hello"),
                new DummyFile(2, "world"),
                new DummyFile(3, "java"));
    }

    class DummyFile {
        int id;
        String content;

        public DummyFile(int id, String content) {
            this.id = id;
            this.content = content;
        }

        public int getId() {
            return id;
        }

        public String getContent() {
            return content;
        }
    }
}
```

The above program will create three text files.

```
C:\\test\\1.txt
hello
```
```
C:\\test\\2.txt
world
```
```
C:\\test\\3.txt
java
```

The **Files.write** may throws **IOException**, and we must catch the exception inside the **forEach**; thus, the code looks ugly. The common practice is to extract the code to a new method.

```java
public void save(Path path, List<DummyFile> files) {

    if (!Files.isDirectory(path)) {
        throw new IllegalArgumentException("Path must be a directory");
    }

    // extract it to a new method
    /*files.forEach(f -> {
          try {
              int id = f.getId();
              // create a filename
              String fileName = id + ".txt";
              Files.write(path.resolve(fileName),
                      f.getContent().getBytes(StandardCharsets.UTF_8));
          } catch (IOException e) {
              e.printStackTrace();
          }
      });*/

    // nice!
    files.forEach(f -> saveFile(path, f));

}

public void saveFile(Path path, DummyFile f) {
    try {
        int id = f.getId();
        // create a filename
        String fileName = id + ".txt";
        Files.write(path.resolve(fileName),
                f.getContent().getBytes(StandardCharsets.UTF_8));
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

Now, we also can write code like this:

```java

ForEachWriteFile obj = new ForEachWriteFile();

  Path path = Paths.get("C:\\test");
  obj.createDummyFiles().forEach(o -> obj.saveFile(path, o));
```

## 5. forEach vs forEachOrdered

The forEach does not guarantee the stream’s encounter order, regardless of whether the stream is sequential or parallel. The result is obvious when run in a parallel mode.

```java
Stream<String> s = Stream.of("a", "b", "c", "1", "2", "3");
  s.parallel().forEach(x -> System.out.println(x));
```

Each run will generate different result:

```
Output:

1
2
b
c
3
a
```

The **forEachOrdered** guarantees the stream’s encounter order; thus, it sacrifices the benefit of parallelism.

```java
 Stream<String> s = Stream.of("a", "b", "c", "1", "2", "3");
  // keep order, it is always a,b,c,1,2,3
  s.parallel().forEachOrdered(x -> System.out.println(x));
```

```
Output:

a
b
c
1
2
3
```