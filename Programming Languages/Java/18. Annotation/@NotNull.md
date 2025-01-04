The `@NotNull` annotation in Java is a constraint annotation used to indicate that a particular field, method parameter, or return value must not be `null`. It is part of the **Bean Validation API** (JSR 380) and is commonly used in frameworks like Hibernate Validator to enforce validation rules.
### **Purpose**

- The `@NotNull` annotation ensures that the value of a variable, method parameter, or return type is not `null`.
- It helps in validating input values and ensuring data integrity, especially in applications with strict validation requirements.
### **Usage**

The `@NotNull` annotation can be applied to:

- Fields
- Method parameters
- Method return values

Example:

```java
import jakarta.validation.constraints.NotNull;

public class User {
    @NotNull(message = "Name cannot be null")
    private String name;

    @NotNull
    private Integer age;

    public User(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(@NotNull String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(@NotNull Integer age) {
        this.age = age;
    }
}
```

### **Validation in Action**

To enforce the validation rules, a validation framework (like Hibernate Validator) is typically used. Example:

```java
import jakarta.validation.Validation;
import jakarta.validation.Validator;
import jakarta.validation.ValidatorFactory;
import jakarta.validation.ConstraintViolation;

import java.util.Set;

public class Main {
    public static void main(String[] args) {
        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        Validator validator = factory.getValidator();

        User user = new User(null, 25); // `name` is null, which violates @NotNull

        Set<ConstraintViolation<User>> violations = validator.validate(user);

        for (ConstraintViolation<User> violation : violations) {
            System.out.println(violation.getMessage());
        }
    }
}
```

Output:

```
Name cannot be null
```

### **Key Points**

1. **Runtime Behavior**:
    
    - `@NotNull` is a **declarative constraint** that requires a validation engine to process it. Without validation processing, it will not automatically enforce constraints.
2. **Validation Message**:
    
    - You can specify a custom message using the `message` attribute.
    - Example: `@NotNull(message = "Field cannot be null")`
3. **Use Cases**:
    
    - Validating data in Java Beans.
    - Ensuring that method arguments are not `null`.
    - Enforcing non-nullable values in APIs.
4. **Difference from `@Nullable`**:
    
    - `@Nullable` is often used as documentation or guidance for developers to indicate that a value can be `null`.
    - `@NotNull` enforces constraints through validation.

### **Limitations**

- Does not work by itself; you need a validation framework to enforce the rules.
- Does not prevent compile-time assignment of `null`.

Using `@NotNull` is a good practice for writing robust, self-documenting, and error-resistant code.