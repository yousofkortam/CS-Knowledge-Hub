# @Entity - Declare entities

One of the great advantages of JPA is that it allows us to manipulate the database through objects, these objects are known as Entity, which are common and current classes also called POJO's (Plain Old Java Objects), these classes have the particularity Since they are classes that are mapped against a table in the database, this mapping is generally carried out using Annotations. These annotations provide enough metadata to be able to relate the classes against the tables and the properties against the columns. It is in this way that JPA is able to interact with the database through classes.

Entities are POJOs so they are simple Java classes like any other, however, JPA must be able to identify which classes are Entities in order to manage them. This is where the importance of the @Entity annotation is born, this annotation must be defined at the class level and serves only to indicate to JPA that this class is an Entity

```java
@Entity
public class Employee {
    private Long id;
    private String name; 

    /**
     * getters and setters
     */
}
```

At this point the class can already be considered an Entity, but it would take one more step for JPA to load this class as an Entity. All the entity classes that we are going to use in our project must be contained in the persistence unit (PersistenceUnit) that we define in the persistence.xml file, so we will have to return to this file and add the class class, for this we will go to the Design tab and press the Add Class button, this will show us a small dialog with all the entities of our project, we select the Employee class and press OK.