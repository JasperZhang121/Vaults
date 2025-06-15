### Package

`org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean`

### Purpose

Marks a `@Configuration` class or `@Bean` method so that it’s only applied when no existing bean of the specified type or name is already present in the Spring `ApplicationContext`. This lets you provide fallback or default beans without overriding user-defined ones.

### When to Use
- **Auto-configuration**: In library or framework code to supply default implementations that can be overridden by the user.
- **Modular setups**: When you want to register a bean only if another module or user configuration hasn’t already provided it.

### Core Attributes
- `value` / `type` (alias): One or more classes; the condition passes if **no** bean of any of these types is present.
- `name`: One or more bean names; the condition passes if **no** bean with any of these names exists.
- `annotation`: One or more annotations; the condition passes if **no** bean annotated with any of these annotations exists.
- `search`: Defines the search strategy (`SearchStrategy.ALL`, `CURRENT`, `EXTENDED`).

### How It Works

1. **Evaluation**: Before creating the bean (or processing the configuration class), Spring’s condition evaluator scans the context for matching beans.
2. **Match Failure**: If a matching bean **is** found, Spring skips registering the annotated bean or configuration entirely.
3. **Match Success**: If none are found, Spring proceeds to register your bean.

Under the hood, Spring implements this via the `Condition` interface, checking the `BeanFactory` for existing definitions.

### Java Examples

```java
// 1. On a @Configuration class
@Configuration
@ConditionalOnMissingBean(DataSource.class)
public class DefaultDataSourceConfiguration {

    @Bean
    public DataSource dataSource() {
        // Provide a default DataSource if none is defined
        return new HikariDataSource();
    }
}
```

```java
// 2. On a specific @Bean method by bean name
@Configuration
public class CacheConfiguration {

    @Bean
    @ConditionalOnMissingBean(name = "cacheManager")
    public CacheManager defaultCacheManager() {
        return new ConcurrentMapCacheManager();
    }
}
```

```java
// 3. Combining type and name
@Configuration
public class ServiceConfiguration {

    @Bean
    @ConditionalOnMissingBean(value = {UserService.class}, name = "userService")
    public UserService fallbackUserService() {
        return new DefaultUserService();
    }
}
```

### Interaction with Other Conditions

You can combine with other `@Conditional*` annotations for fine-grained control. For example:

```java
@Bean
@ConditionalOnMissingBean(EmailSender.class)
@ConditionalOnProperty(name = "app.email.enabled", havingValue = "true")
public EmailSender defaultEmailSender() {
    return new SmtpEmailSender();
}
```

Here, the bean loads only if email support is enabled **and** no other `EmailSender` bean exists.
