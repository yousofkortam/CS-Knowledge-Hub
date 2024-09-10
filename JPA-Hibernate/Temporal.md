# @Temporal - Date mapping

Using the @Temporal annotation it is possible to map the dates with the database in a simple way. One of the main complications when working with date and time is determining the format used by the database manager. However, this will no longer be a problem with @Temporal.

By using @Temporal it is possible to determine if our attribute stores Time, Date or Time and date, and it is possible to use the Date or Calendar class for these purposes

It is advisable to use Calendar as it has many more operations to manipulate date and time.

You can set three possible values ​​for the annotation:

- **DATE**: It will limit the field only to the Date, discarding the time.
```@Temporal (TemporalType.DATE)```
- **TIME**: It will limit the field only to the Time, discarding the date.
```@Temporal (TemporalType.TIME)```
- **TIMESTAMP**: Take the date and time.
```@Temporal (TemporalType.TIMESTAMP)```

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
    
    @Column(name = "INGRESS_DATE", updatable = false, nullable = false)
    @Temporal(TemporalType.DATE)
    private Calendar registDate;
     
    /**
     * getters and setters
     */
}
```






