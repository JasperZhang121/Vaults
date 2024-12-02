`@TableName` is an annotation provided by **MyBatis-Plus**, which is a powerful tool that simplifies the use of MyBatis in Java applications. This annotation is typically used on entity classes to map them to database tables. The main purpose of `@TableName` is to specify the name of the database table that an entity class is associated with, especially when the table name does not follow MyBatis-Plus's default naming conventions.

### Example:

```java
import com.baomidou.mybatisplus.annotation.TableName;

@TableName("user_info")
public class User {
    private Long id;
    private String username;
    private String email;
    
    // Getters and setters
}
```

### Key Points:

- **@TableName** can be used to specify the exact table name in the database if it differs from the class name. For example, in the example above, `User` is mapped to the `user_info` table.
- If no `@TableName` annotation is used, MyBatis-Plus will assume the class name (in this case `User`) corresponds to a table named `user` by default, using a lowercase version of the class name.

### Optional Attributes:

- **value**: The actual table name in the database (this is the most common usage).
- **autoResultMap**: A boolean flag that indicates whether to use the automatic result map feature (useful for more complex mappings).

### Example with `autoResultMap`:

```java
@TableName(value = "user_info", autoResultMap = true)
public class User {
    private Long id;
    private String username;
    private String email;
    
    // Getters and setters
}
```

This setup allows you to leverage MyBatis-Plus's automatic features, simplifying your database operations while also enabling flexibility when your table names don't follow typical naming conventions.