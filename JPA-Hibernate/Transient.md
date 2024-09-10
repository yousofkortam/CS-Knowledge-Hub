# @Transient - volatile attributes

The @Transient annotation is used to tell JPA that an attribute of an Entity should not be persistent, in this way, JPA ignores the attribute and is not taken into account when persisting the Object.

In practice it is not common to use this annotation, because Entities generally only have the attributes that map to the database. However, there are times when it can be helpful. A very common example is when we add a logger to the entity class. This instance of java.util.logging.Logger will be one more property of the entity, but we do not want it to be persistent, for which we mark it with @Transient.

```java
import java.util.Calendar;
import java.util.Objects;
import java.util.logging.Logger;
import javax.persistence.*;


@Entity
@Table(
    name = "CUSTOMERS" , 
    schema = "examplejpa", 
    indexes = {@Index(name = "name_index", columnList = "name",unique = true)}
)
public class Customer {
    
    @Transient
    private static final Logger logger = Logger.getLogger(Customer.class.getSimpleName());
    
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
    private Calendar registDate;
    
    @Column(name="STATUS", nullable = false, length = 8 )
    @Enumerated(value = EnumType.STRING)
    private Status status;
     
    /**
     * getters and setters
     */
}
```

```java
public enum Status {
    ACTIVE,
    INACTIVE
}
```





