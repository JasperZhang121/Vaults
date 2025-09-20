The `@RequiredArgsConstructor` annotation is part of the Lombok library. It automatically generates a constructor for your class that takes as parameters all fields that are either:
	•	<mark style="background: #FFB8EBA6;">final fields</mark>: Because they must be initialized.
	•	Fields annotated with @NonNull: Ensuring they are not null (unless they have default values).

This helps reduce boilerplate code, especially when you have many final or required fields in your class.

### Example

```java
import lombok.NonNull;
import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
public class User {
    private final int id;
    @NonNull
    private String name;
    private int age; // Not final and not @NonNull, so it's not included in the constructor.
}

The annotation above will generate a constructor equivalent to:

public User(int id, @NonNull String name) {
    this.id = id;
    this.name = name;
}
```

Notice that age is not included because it’s neither final nor marked with @NonNull.

Benefits
	•	Less Boilerplate: You don’t have to manually write constructors.
	•	Immutability: Helps in creating immutable objects by enforcing initialization of final fields.
	•	Cleaner Code: Reduces clutter, making your code easier to read and maintain.
