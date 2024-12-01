The `@Conditional` annotation in Spring is used to conditionally enable or disable beans based on certain conditions. It allows you to define conditions that must be met for a bean to be registered in the application context.

Here's an overview of how `@Conditional` works:

### Purpose:

`@Conditional` is used to create custom conditions for when a particular bean should be registered. For example, you can enable a bean only if a certain property is set, or if a specific class is present in the classpath.

### Key Points:

- `@Conditional` requires a class that implements the `Condition` interface.
- This condition class checks whether the bean should be created or not based on a condition you define.

### How it works:

1. **Condition Interface:** You create a class that implements the `Condition` interface and override the `matches` method. This method returns `true` if the condition is met, and `false` if the condition is not met.
    
2. **Apply `@Conditional`:** Apply the `@Conditional` annotation to the `@Bean` or class that you want to conditionally load.

### Example:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyConfiguration {

    @Bean
    @Conditional(MyCondition.class)
    public MyBean myBean() {
        return new MyBean();
    }
}

class MyCondition implements Condition {

    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        // Custom logic to check condition
        // For example, check if a specific property is set
        String myProperty = context.getEnvironment().getProperty("my.custom.property");
        return "enabled".equals(myProperty);
    }
}

class MyBean {
    // Bean implementation
}
```

### Explanation:

1. **`MyCondition`**: This class implements the `Condition` interface and overrides the `matches` method. It contains the logic to check if a condition is met. In this example, it checks if a custom property (`my.custom.property`) is set to "enabled".
    
2. **`@Conditional(MyCondition.class)`**: This annotation tells Spring to only create the `myBean` bean if the `MyCondition` class's `matches` method returns `true`. If the condition fails (e.g., the property is not set to "enabled"), the bean will not be registered.

### Use Cases:

- **Profile-based configuration**: You can define beans to be loaded only under specific conditions, such as specific profiles or properties.
- **Class presence check**: Load beans only when certain classes or resources are available on the classpath.
- **Custom conditions**: You can create complex logic to decide when to load a bean based on any conditions.

This gives you fine-grained control over your bean configuration in Spring, enabling or disabling beans based on any context-specific conditions.