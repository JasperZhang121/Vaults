In MyBatis, the `@Mapper` annotation is used to designate an interface as a Mapper. A Mapper is essentially an interface that MyBatis uses to map SQL statements to Java methods. When you annotate an interface with `@Mapper`, MyBatis will automatically generate the implementation for the interface at runtime, making it easier to interact with the database without manually writing the implementation.

Here’s an example of how to use `@Mapper`:

### Example:

```java
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface UserMapper {
  
    @Select("SELECT * FROM users WHERE id = #{id}")
    User findById(int id);
}
```

In this example:

- The `@Mapper` annotation tells MyBatis to treat `UserMapper` as a Mapper.
- The `@Select` annotation is used to define a simple SQL query.

When the application runs, MyBatis will create a proxy object for the `UserMapper` interface, and the actual database operations are handled by MyBatis under the hood.

### Benefits of `@Mapper`:

1. **Automatic Mapping**: You don't need to manually implement SQL operations.
2. **Declarative Queries**: You can use annotations like `@Select`, `@Insert`, `@Update`, `@Delete`, etc., to define SQL statements.
3. **Simplified Development**: With `@Mapper`, you reduce boilerplate code.

### Usage in Spring Boot:

If you're using Spring Boot with MyBatis, make sure that your configuration class is set up correctly to scan for Mapper interfaces.

```java
@SpringBootApplication
@MapperScan("com.example.mapper")  // Scan the package where Mappers are located
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

In this case, the `@MapperScan` annotation tells Spring Boot to scan the specified package for Mapper interfaces, and the application will automatically recognize `@Mapper` annotations on interfaces.