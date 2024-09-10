# @EmbeddedId - Embed primary key

Another way to define composite primary keys is with the @EmbeddedId Enumeration, which allows you to mark a class as ID. Unlike @IdClass, this method does not require defining the attributes of the primary key in the entity, but only needs to add the class that contains all the fields as an attribute.

One difference that this method has with respect to @IdClass, is that it is necessary for the ID class to be annotated at the class level with the @Embeddable annotation. This tells JPA that this class can be embedded within another.

We are going to take back the entity Telephone and the class TelephonePK and modify them that instead of using @Id, use @EmbeddedId. We will modify the TelephonePK class and it would be as follows:

```java
import java.util.Objects;
import javax.persistence.Embeddable;

@Embeddable
public class TelephonePK {
    private Long employeId;
    private String telType;

    @Override
    public int hashCode() {
        int hash = 8;
        hash = 59 * hash + Objects.hashCode(this.employeId);
        hash = 59 * hash + Objects.hashCode(this.telType);
        return hash;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        final TelephonePK other = (TelephonePK) obj;
        if (!Objects.equals(this.telType, other.telType)) {
            return false;
        }
        if (!Objects.equals(this.employeId, other.employeId)) {
            return false;
        }
        return true;
    }
    /** getters and setters*/
}
```

Second, we will modify the entity Telephone and it would be as follows:

```java
import com.co.alejo.TelephonePK;
import javax.persistence.EmbeddedId;
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "Telephones")
public class Telephone {
    
    @EmbeddedId
    private TelephonePK id;
    
    private String number;

    /** getters and setters*/
}
```

First of all, let's note that we have removed the employeeId and telType properties to be replaced by the TelephonePK case, to which we add the @EmbeddedId annotation.

The @EmbeddedId annotation will cause the fields defined in the TelephonePK class to be treated as if they were part of the Telephone class.

It is worth mentioning that both @IdClass and @EmbeddedId will give the same result at runtime, so the only difference is only at the code level. Remember that with @IdClass it is necessary to define the fields that will be the primary key both in the Entity and in the Id classes, on the other hand, with @EmbeddedId it will only be necessary to embed the key as one more property of the case. I particularly prefer to work with @EmbeddedId to avoid repeating attributes.