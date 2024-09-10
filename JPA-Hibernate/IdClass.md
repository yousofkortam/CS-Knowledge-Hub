# @IdClass - Composite keys

There are occasions where it is required to mark more than one field as @Id, thus conforming a composite primary key. In these cases it is required to complement the entity with an additional class that will be used as ID and it will also have to be referenced from the class where a composite key is required.

Using @IdClass is one of two options for defining composite primary keys, and this consists of creating an additional class with only the fields that correspond to the primary key.

Let's see a specific case, normally an employee can have more than one phone, so we could create a table where the primary key is the employee ID and the type of phone, in this way we make sure we have only one type of phone per employee

```java
import java.util.Objects;
import javax.persistence.Id;

public class TelephonePK {
    private Long employeId;
    private String telType;

    @Override
    public int hashCode() {
        int hash = 7;
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

We define the class TelephonePK, which only has as attributes the type of telephone and the id of the employee. Note that this does not require any type of annotation, but it is required to override the hashCode & equals methods.
The next class we define is the Telephone Entity which represents an employee telephone.

```java
import com.obb.jpa.jpaturorial.entity.pk.TelephonePK;
import java.util.Objects;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.IdClass;
import javax.persistence.Table;


@Entity
@Table(name = "Telephones")
@IdClass(value = TelephonePK.class)
public class Telephone {
    @Id
    private Long employeId;
    @Id
    private String telType;
    
    private String number;

    /** getters, setters, HashCode & Equals */
}
```

Let us observe that this class has the employee's Id and the type of telephone marked as @Id, in addition, it has the annotation @IdClass at the class level and as a value it has the EmployeePK class.
An important piece of information is that both the definition of the attributes in the ID class and in the Entity class must match perfectly, otherwise it will cause an error at runtime.

```sql
TELEPHONES
pk  employeeId
PK  telType
    number
```