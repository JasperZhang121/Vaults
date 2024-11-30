
The `@Builder` annotation, provided by **Lombok**, is used to implement the **Builder design pattern** for your Java classes. This pattern allows for creating <mark style="background: #FFB8EBA6;">immutable objects or complex objects with multiple optional parameters in a clean and readable way</mark>.

#### What It Does:

When applied to a class or constructor, Lombok generates a nested static `Builder` class with methods for setting values for each field and a `build()` method to create an instance of the target class.

#### When to Use:

Use `@Builder` when:

1. You want to build objects with many optional parameters.
2. You want to create immutable objects (often combined with `@AllArgsConstructor` and private constructors).
3. You want to make object creation more readable and less error-prone compared to constructors with many parameters.

#### Example:

##### Without Lombok:

Here’s how you’d manually implement the Builder pattern:

```java
public class User {
    private final Long id;
    private final String name;
    private final String email;

    private User(Builder builder) {
        this.id = builder.id;
        this.name = builder.name;
        this.email = builder.email;
    }

    public static class Builder {
        private Long id;
        private String name;
        private String email;

        public Builder id(Long id) {
            this.id = id;
            return this;
        }

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

Usage:

```java
User user = new User.Builder()
                 .id(1L)
                 .name("John Doe")
                 .email("john.doe@example.com")
                 .build();
```

##### With Lombok:

Using `@Builder`, the entire manual implementation is replaced with a simple annotation:

```java
import lombok.Builder;
import lombok.ToString;

@Builder
@ToString
public class User {
    private Long id;
    private String name;
    private String email;
}
```

Usage:

```java
public class Main {
    public static void main(String[] args) {
        User user = User.builder()
                        .id(1L)
                        .name("John Doe")
                        .email("john.doe@example.com")
                        .build();

        System.out.println(user);
    }
}
```

Output:

```
User(id=1, name=John Doe, email=john.doe@example.com)
```

#### Advanced Features:

- **Custom Builders**: You can create customized builder methods by specifying fields for the builder.
- **@Builder(toBuilder = true)**: Adds a `toBuilder()` method to the class, which allows you to create a pre-populated builder from an existing object.

Example with `toBuilder`:

```java
@Builder(toBuilder = true)
public class User {
    private Long id;
    private String name;
    private String email;
}

User user = User.builder().id(1L).name("John").build();
User updatedUser = user.toBuilder().email("john.new@example.com").build();
```

#### When Not to Use:

- Avoid `@Builder` for simple classes with only a few fields, as it might be overkill.
- Be cautious when using `@Builder` with required fields; it does not enforce compile-time checks for required fields. Use `@NonNull` for required fields to throw an exception at runtime.

#### Summary:

- **What it is:** A Lombok annotation to implement the Builder pattern.
- **When to use:** For creating complex objects with many optional parameters or immutable objects.
- **Example:**
    
    ```java
    @Builder
    public class Employee {
        private Long id;
        private String name;
        private String department;
    }
    
    Employee emp = Employee.builder().id(101L).name("Alice").build();
    ```