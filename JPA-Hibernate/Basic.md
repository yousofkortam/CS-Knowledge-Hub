# @Basic - loading strategy

@Basic is an annotation that allows us to control when a property is loaded from the database, avoiding bringing values ​​that are not necessary when loading the object. This annotation is generally used to annotate heavy objects, such as an image or a binary file.


In JPA there are two concepts that are key to understanding how JPA loads objects from the database and these are key to improving the performance of the application, these concepts are explained below:

- **Lazy loading**: Lazy loading objects will not be loaded from the database when the object is created, but will be loaded as soon as the property is accessed. In this way, JPA identifies when the property is accessed for the first time to load the value from the database.
    -  ```@Basic (fetch = FetchType.LAZY)```
- **Eager loading (Eager or early loading)**: This is the one used by default for most of the properties in JPA, with the exception of the collections which we will analyze later.
    - ```@Basic (fetch = FetchType.EAGER)```

The @Basic annotation also defines the optional property, which is of the Boolean type, if true is indicated, we tell JPA that this property is optional and there may be a Null value in it, if it is false, we indicate that the property must always have value. This property is mostly to help JPA optimize the queries to get the values. But it will have to correspond with reality, since in case of marking that it is not optional and this value is null in the JPA database it marks an error.

```NOTE```: optional is different from nullable of @Column, since nullable is used to define the structure of the column and optional are suggestions to JPA to optimize the querys. We should never confuse that.

An important point is that @Basic can be used for practically anything like primitive types, wrapper classes, Calendar, enums, Time, Date, Timestamp. byte [], char [], classes that implement java.lang.Serializable, etc.

```java
import java.util.Calendar;
import java.util.Objects;
import javax.persistence.*;

@Entity
@Table(
    name = "PROFILE" , 
    schema = "lobexample", 
    indexes = {@Index(name = "name_index", columnList = "name",unique = true)}
)
public class Profile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY, generator = "ProfileTable")
    @Column(name = "ID")
    private Long id;
    
    @Column(name = "NAME", nullable = false, length = 100)
    private String name; 
    
    @Column(name = "PHOTO" , nullable = true)
    @Basic(optional = false, fetch = FetchType.EAGER)
    @Lob()
    private byte[] photo;
    
    /**
     * getters and setters
     */
}
```

