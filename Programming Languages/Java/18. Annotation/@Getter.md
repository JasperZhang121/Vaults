
`@Getter` is a Lombok annotation used to generate getter methods for the fields of a class. It reduces boilerplate code by eliminating the need to manually write getter methods.

#### Example:

```java
import lombok.Getter;

public class User {
    @Getter
    private String name;

    @Getter
    private int age;
}
```

**Generated Code (Equivalent)**:

```java
public class User {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

**Usage**:

```java
User user = new User();
user.getName(); // Getter method generated automatically by Lombok
```
