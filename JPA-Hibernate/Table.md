# @Table - Table mapping

We will talk about the @Table annotation and its importance when defining entities. The @Table annotation is used to tell JPA against which table it should map an entity, in this way when a persistence, deletion or select of the entity is performed, JPA will know against which table in the database it should

This annotation is used to indicate the table against which the entity maps, but it also has other interesting properties like the ones shown below:

- **name**: it is used to put the real name of the table in the database, it is recommended that the name be exact respecting upper and lower case, especially when we work on Linux.
- **schema**: it is used to indicate the schema in which the table is located. This property is generally not necessary, unless the table is in a different schema than the one we use to log in.
- **Indexes**: JPA allows us to indicate the indexes that our table has, this option becomes relevant when we indicate that JPA creates the tables for us (we will see this feature later).

```java
import java.util.Calendar;
import javax.persistence.*;

@Entity
@Table(
    name = "EMPLOYEES" , 
    schema = "employeeschema", 
    indexes = {@Index(name = "name_index", columnList = "name",unique = true)}
)
public class Employee {
    private Long id;
    private String name; 

    /**
     * getters and setters
     */
}
```

Let's notice that we have defined that the Employee class maps against the EMPLOYEES table, we tell it that the table is in the employeeschema schema and that it has an index called name_index to sort the employee's name. In the case of the index for the employee's name, we are saying that the name must be unique

```Important```: The @Table notation is not mandatory, but the fact of not putting it means that JPA determines that the table is called the same as the class and will look for it in the schema in which you are logged.

## Schema auto generation
One of the great advantages of JPA is that it can create all database objects such as Tables and Sequences automatically using metadata. This behavior is defined in the persistence.xml file, let's go back to this file and see that it has a property called **Table Generation Strategy** which has the following possible values:

- **Create**: We tell JPA that if an object does not exist, it creates it, but if it already exists, it will not be created or updated. One detail to take into account is that if we add a new field to an entity and its table already exists, this new field will not be created, so we will have to manually update the table or delete it to create it again when instantiating. the EntityManager.
- **Drop and Create**: This strategy deletes all the objects to recreate them, this strategy guarantees that all the objects in the database are updated with the entities. DANGER: This strategy is very dangerous, since it will erase all the information that we have in the database and it will be impossible to recover it, which would be fatal if we execute it in a productive environment.
- *None**: With this option we tell JPA not to do anything, that is, not to create or delete anything. This option is the best when we already have the entire database model developed in the database. Or when we connect to an existing model.

I suggest you start using the Create strategy so as not to worry about creating the objects, on the other hand, my advice is never to use Drop and create because it is too risky.

Returning to the index property of @Table, this property only serves to create the table and does not have any operation at runtime, so if you are working with the None strategy, then there is no point in defining this property.