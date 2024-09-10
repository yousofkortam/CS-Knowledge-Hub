# @Lob - heavy objects

JPA allows us, through the @Lob annotation, to map heavy objects with the database, such as images, xml, binaries, long text strings, json, etc. Any object that can be very large or of indefinite length

The @Lob annotation is all that is required to tell JPA that this field is a heavy object and should be treated as such. It is generally used with byte arrays, as it allows you to store anything.

The @Lob annotation does not have any attributes, so it will only need to be defined for it to work.

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
    
    
    @Column(name = "PHOTO" ,nullable = true)
    @Basic(optional = false, fetch = FetchType.EAGER)
    @Lob()
    private byte[] photo;
    
    /**
     * getters and setters
     */
}
```

@Lob is much simpler than it seems, however, remember that the @Column & @Basic annotations will always be necessary to define the column attributes and the loading strategy. In general, the properties marked as @Lob are loaded when the object is created and the heavy object is not always required, so it is advisable to use the @Basic annotation and mark the attribute as LAZY.





