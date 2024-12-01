In Java, the `@Table` annotation is used to specify the name of the database table that an entity class will be mapped to when using JPA (Java Persistence API). This is typically used alongside the `@Entity` annotation to define the relationship between a class and a database table.

### Example Usage of `@Table`:

```java
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "employee")
public class Employee {
    private Long id;
    private String name;
    private String position;

    // Getters and setters
}
```

### Attributes of `@Table`:

- `name`: Specifies the name of the database table. If not specified, JPA uses the name of the entity class as the table name (with possible adjustments depending on the naming strategy).
- `schema`: Specifies the schema in which the table is located (optional).
- `catalog`: Specifies the catalog name (optional).
- `uniqueConstraints`: Defines unique constraints on columns (optional).

### Example with Unique Constraints:

```java
@Entity
@Table(name = "employee", uniqueConstraints = {
    @UniqueConstraint(columnNames = {"email"})
})
public class Employee {
    private Long id;
    private String name;
    private String email;

    // Getters and setters
}
```

The `@Table` annotation helps you map your Java entity class to an existing table in the database, providing flexibility in naming and constraint configurations.