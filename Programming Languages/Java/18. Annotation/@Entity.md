In Java, the `@Entity` annotation is used to mark a class as an entity in JPA (Java Persistence API). This means that the class is mapped to a database table, and JPA will manage the persistence of its instances in the database.

Example of how to use `@Entity`:

```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class Employee {

    @Id
    private Long id;
    private String name;
    private String department;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }
}
```

### Key Points:

1. **`@Entity`**: Marks the class as a JPA entity.
2. **`@Id`**: Marks the primary key of the entity. It can be applied to a field or a method.
3. **Table Mapping**: The class is automatically mapped to a table named the same as the class name (`Employee` in this case), but you can customize the table name with the `@Table` annotation if needed.

You can now use this class with a `EntityManager` or a `Repository` to perform CRUD operations on the corresponding table in your database.