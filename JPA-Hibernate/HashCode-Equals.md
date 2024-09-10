# HashCode and Equals

By default, all objects in Java inherit from the Object case the hashCode and equals methods which serve to identify if two variables refer to the same object.
The de facto behavior of the hashCode method returns the position in memory of an object, and the equals method compares the hashCode of the two objects evaluated, in this way, if the two variables refer to the same memory position, then it is said that they are the same, otherwise they are different.
In the case of Entities, the default implementation of these methods does not work correctly, because one Entity says that it is equal to another if two conditions are met:

- The two objects are of the same class.
- The value of your ID (@Id) are the same.

If these two conditions are met then the two entities are equal regardless of not referring to the same object in memory. Because of this, it is important to override these two methods so that they evaluate an Entity for the two conditions mentioned.

```java
import java.util.Objects;
import javax.persistence.*;

@Entity
@Table(
    name = "EMPLOYEES" , 
    schema = "employeeschema", 
    indexes = {@Index(name = "name_index", columnList = "name",unique = true)}
)
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY, generator = "EmployeeTable")
    private Long id;
    private String name; 

    @Override
    public int hashCode() {
        int hash = 7;
        hash = 29 * hash + Objects.hashCode(this.id);
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
        final Employee other = (Employee) obj;
        if (!Objects.equals(this.id, other.id)) {
            return false;
        }
        return true;
    }

     /**
     * getters and setters
     */
}
```

The hashCode method will return a hash generated from the ID, let's see that a hash variable with value 7 intervenes in the equation. Don't worry about that, it's just a randomly generated number.

On the other hand, the equals method will first compare the two objects with the == operator, this operator translates into the hashCode method call in such a way that this == obj would be equal to this.hashCode () == obj .hashCode (), and as we have seen, the hashCode is generated from the ID and not from the memory position, for this reason the expression would only be true if both objects have the same