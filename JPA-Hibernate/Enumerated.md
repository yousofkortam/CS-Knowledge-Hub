# @Enumerated

One of the advantages of using enumerations in Java, is we can limit the possible values ​​for a property, forcing developers to use the values ​​already defined and avoiding the margin of error.

Enums can also be used with JPA and they can be very helpful in ensuring that programmers persist a valid value within a previously defined list. JPA allows us through the @Enumerated annotation to define the way in which an enumeration will be persisted, which are explained below:

- **String**: allows the enumeration to be persisted by name, which means it will be an alphanumeric column. The annotation would look like this:
```@Enumerated (value = EnumType.STRING)```
- **Ordinal**: This strategy persists an integer value that corresponds to the ordinal value or value position in the enumeration. The annotation would be as follows:
```@Enumerated (value = EnumType.ORDINAL)```

The **@Enumerated** annotation is not mandatory, but omitting it will cause JPA to determine the default strategy, which in our case would be Ordinal

```java
import java.util.Calendar;
import java.util.Objects;
import javax.persistence.*;

@Entity
@Table(
    name = "CUSTOMERS" , 
    schema = "examplejpa", 
    indexes = {@Index(name = "name_index", columnList = "name",unique = true)}
)
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY, generator = "CustomerTable")
    @Column(name = "ID")
    private Long id;
    
    @Column(name = "NAME", nullable = false, length = 100)
    private String name; 
    
    @Column(name = "SALARY", nullable = false, scale = 3)
    private double salary;
    
    @Column(name = "INGRESS_DATE", updatable = false, nullable = false)
    @Temporal(TemporalType.DATE)
    private Calendar ingressDate;
    
    @Column(name="status", nullable = false, length = 8 )
    @Enumerated(value = EnumType.STRING)
    private Status status;
    
    /**
     * getters and setters
     */
}
```

Status enumeration which has the possible ACTIVE and INACTIVE values

```java
public enum Status {
    ACTIVE,
    INACTIVE
}
```
