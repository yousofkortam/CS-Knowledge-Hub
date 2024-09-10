# @Column - Definition of columns

One of the main characteristics when we work with databases is that all tables have Columns, and these columns are mapped against the attributes of the entities, for which it is necessary for JPA to identify which column maps against each attribute of the class and is this is where @Column comes in.

The @Column annotation will allow us to define very important aspects about the database columns of the database such as the name, the length, constrains, etc. In case of not defining this annotation in the attributes, JPA will determine the name of the column automatically through the name of the attribute, so it is always advisable to establish this annotation in all the attributes of the class and avoid problems.

The properties that we can define to @Column are the following:

- **name**: It allows to establish the name of the column of the table with which the attribute must map. Even though no @Column attribute is required, I always recommend setting this attribute.
- **length**: Allows you to define the length of the column in characters, it only applies to Strings, in the other data types it will be omitted. It is only used when JPA will create the tables.
- **insertable**: It tells JPA if that column should be taken into account in the inserts, if it is true, the value will be inserted, otherwise the value will be omitted and the default value of the column or null will be placed.
- **updatable**: Similar to the previous case, only in this case the column is taken into account for Update operations.
- **nullable**: Creates a constraint on the table (Not Null) to prevent null values ​​from being inserted. It is only used when JPA will create the tables.
precision:
- **scale**: Used only for columns that must have decimals, as value it receives the number of decimal places. It is only used when JPA will create the tables.
- **table**: This property is very advanced and we will discuss it later. It is used when there is inheritance in the entities and the columns can be distributed in more than one table.
- **unique**: It will create a restriction on the table so that the value of that column is unique. It is only used when JPA will create the tables.

```java
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
    
    @Column(name = "NAME", nullable = false, length = 100)
    private String name; 
}
```

Let's see that the id attribute only requires defining the name property, because by default, it is already unique, not null, not updatable. So with defining the name it will be necessary.

In the case of the name, we define the name property, we indicate that it cannot be null and we are defining a length of 100 characters.

