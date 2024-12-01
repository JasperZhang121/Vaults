The `@Min` annotation in Java is used to specify the minimum value for a numeric field, often in validation contexts. It's part of the `javax.validation.constraints` package, which is included in the Bean Validation API. When you apply `@Min` to a field, it ensures that the value of the field is greater than or equal to the specified minimum value. If the value is lower, it triggers a validation error.

Here's an example of how to use `@Min`:

```java
import javax.validation.constraints.Min;

public class Product {
    
    @Min(value = 0, message = "Price must be at least 0")
    private double price;
    
    // Getters and setters
    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

In this example, the `@Min` annotation is applied to the `price` field. The validation will ensure that the price is at least `0`. If a value less than `0` is provided, it will trigger a validation error with the message `"Price must be at least 0"`.

### Common Use Cases

- Validating user input in forms (e.g., ensuring a minimum age or quantity).
- Ensuring that monetary amounts or counts do not fall below certain thresholds.

### Notes

- The `@Min` annotation works with numeric types like `int`, `long`, `double`, and `BigDecimal`.
- This is typically used in combination with a validation framework, such as Hibernate Validator, in a Spring Boot application for automatic validation.