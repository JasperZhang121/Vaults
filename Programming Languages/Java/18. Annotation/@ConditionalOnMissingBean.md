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
```

### Definition and Background

- **Annotation Name**: `@ConditionalOnMissingBean`
- **Framework**: Spring Framework (specifically part of Spring Boot’s auto-configuration mechanism)
- **Import Statement**:
    ```java
    import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
    ```
- **Introduced In**: Available since Spring Boot 1.x, designed to simplify control over auto-configuration precedence.

### Purpose and Intent

The fundamental purpose of `@ConditionalOnMissingBean` is to **register a bean only if there is no existing bean of the same type or name** in the Spring application context. Its primary design goals include:

1. **Supplying a Default Implementation**
    - When writing a library or starter, one can define a “fallback” or “default” bean so that, if the user does not explicitly configure their own bean, they still receive baseline functionality.
        
2. **Allowing User Overrides**
    - If the application context already contains a bean of the specified type (or name), the automatically configured bean is ignored. This ensures that user-defined beans take higher priority.
        
3. **Streamlining Auto-Configuration**
    - Paired with Spring Boot’s `@EnableAutoConfiguration`, it empowers the framework to auto-inject necessary components only when they are absent, avoiding unintended conflicts with user definitions.
        

### Suitable Scenarios

1. **Spring Boot Starter or Third-Party Library**
    - Commonly used in a Starter’s auto-configuration module to provide defaults (for example, a default `DataSource`, `RestTemplate`, `ObjectMapper`, or `CacheManager`).
    - Example: A starter may declare a default `CacheManager` bean if the application has not already defined one.
        
2. **Modular System Design**
    - In a modular architecture, each module can supply its own default implementation. If multiple modules are on the classpath, only the first module lacking a matching bean will contribute its default, and subsequent defaults are skipped.
    - This facilitates a “plug-and-play” component system where modules do not override each other unless explicitly configured.
        
3. **Differing Test and Production Implementations**
    - During testing, developers may register a mock bean (e.g., an in-memory database or a stub service). In production, the default implementation (such as the actual JDBC-based `DataSource`) should be used.
    - By using `@ConditionalOnMissingBean`, the test context’s mock bean prevents the auto-configuration of the default production bean.
        

### Annotation Attributes

`@ConditionalOnMissingBean` supports multiple attributes to fine-tune the lookup condition:

1. **`value` / `type`**
    - Specifies one or more classes. The condition is true only if there is no existing bean of any of those types.    
    - Example:    
        ```java
        @ConditionalOnMissingBean(MyService.class)
        ```
        
2. **`name`**
    - Looks for a bean with a specific bean name (i.e., bean ID). If no bean with that name exists, the condition holds true.   
    - Example:    
        ```java
        @ConditionalOnMissingBean(name = "mySpecialBean")
        ```
        
3. **`parameter` or `field`**
    - When used on a `@Bean` method parameter or a field, Spring checks whether a bean matching that type is already available. If none is found, the current bean definition is allowed.
        
4. **`search` (Search Strategy)**
    - Controls the scope of the lookup:
        - `SearchStrategy.ALL` (default): Searches the current context and all ancestor contexts.    
        - `SearchStrategy.CURRENT`: Searches only the current context, ignoring ancestor contexts. 
        - `SearchStrategy.ANCESTORS`: Searches only ancestor contexts, not the current one.
        - `SearchStrategy.CUSTOM`: Allows custom logic by implementing `BeanFactorySearchStrategy`.

### Mechanism Overview

1. **Startup-Time Scanning**
    - During application startup, Spring Boot scans for configuration classes (`@Configuration`, `@Component`, etc.). When it encounters a `@Bean` method annotated with `@ConditionalOnMissingBean`, it defers registration until after verifying the condition.
        
2. **Condition Evaluation**
    - Before registering the bean, Spring queries the `ApplicationContext` (and, depending on `search`, possibly its parent contexts) for existing beans matching the specified type or name.
    - If no matching bean is found, the condition evaluates to true, and Spring proceeds to register the bean. Otherwise, the bean definition is skipped.
        
3. **Bean Registration**
    - If the condition passes, Spring instantiates and registers the bean in the context as usual.
    - If the condition fails (because a matching bean already exists), Spring omits this bean definition entirely.

### Example Usage

```java
package com.example.autoconfig;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * Demonstrates an auto-configuration class that provides
 * a default implementation of MyService if none is defined.
 */
@Configuration
public class MyServiceAutoConfiguration {

    /**
     * Registers MyServiceImpl only if no other MyService bean exists.
     */
    @Bean
    @ConditionalOnMissingBean(MyService.class)
    public MyService defaultMyService() {
        return new MyServiceImpl();
>>>>>>> origin/main
    }
}
```

<<<<<<< HEAD
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
=======
#### Explanation
- **MyService**: An interface or abstract type defining required operations.
- **MyServiceImpl**: A class implementing `MyService`, serving as the default fallback.
- **Behaviour**:
    - If the application context does not already contain a bean of type `MyService`, Spring will invoke `defaultMyService()` and register its return value (`MyServiceImpl`).
    - If the context already contains a `MyService` bean (for example, a user-provided implementation), Spring ignores `defaultMyService()` and does not register `MyServiceImpl`.


### Comparison with Related Conditional Annotations

|Annotation|Purpose|
|---|---|
|`@ConditionalOnMissingBean`|Registers the bean **only if** no bean of the specified type or name is present in the context.|
|`@ConditionalOnBean`|Registers the bean **only if** a bean of the specified type or name **is already present**.|
|`@ConditionalOnProperty`|Registers the bean **only if** a certain configuration property (in `application.properties`/`yml`) is defined and meets specified criteria.|
|`@ConditionalOnClass`|Registers the bean **only if** a certain class is present on the classpath.|
|`@ConditionalOnResource`|Registers the bean **only if** a specified resource (file or classpath resource) exists.|
|`@ConditionalOnMissingClass`|Registers the bean **only if** a specified class is **not** present on the classpath.|
|`@ConditionalOnExpression`|Registers the bean **only if** a given SpEL expression evaluates to true.|
|`@Conditional` (general)|A generic conditional mechanism; the user implements the `Condition` interface to define any custom logic.|


### Benefits and Considerations

1. **High Customizability**
    
    - Developers can provide default behaviors that “just work” out of the box but remain fully overrideable.
        
2. **Avoids Bean Conflicts**
    
    - Ensures that auto-configuration does not inadvertently override user-defined configurations, reducing the risk of unexpected behavior.
        
3. **Reduces Configuration Overhead**
    
    - Users do not need to explicitly disable auto-configuration; Spring Boot handles it automatically based on presence or absence of beans.
        
4. **Minimal Performance Impact**
    
    - The condition check occurs during startup. Unless the application has an extremely large number of beans or very complex conditional logic, the overhead is negligible.
        
5. **Scope Control via `search`**
    
    - When using parent–child contexts, carefully choose between `SearchStrategy.ALL`, `CURRENT`, or `ANCESTORS` to avoid unintentionally filtering out a valid bean.
        
6. **Combination with Other Conditions**
    
    - For fine-grained control, combine `@ConditionalOnMissingBean` with annotations like `@ConditionalOnProperty` (to only apply the fallback when a property is set), or `@ConditionalOnClass` (to only apply when a certain library is on the classpath).

### Advanced: Search Strategies in Detail

`@ConditionalOnMissingBean` supports a `search` attribute to specify where Spring should look for existing beans:

```java
@ConditionalOnMissingBean(
    value = MyService.class,
    search = SearchStrategy.CURRENT
)
```

- **`SearchStrategy.ALL`** (default)
    
    - Inspects the current `ApplicationContext` and all ancestor contexts.
        
- **`SearchStrategy.CURRENT`**
    
    - Inspects only the current context, ignoring ancestor contexts.
        
- **`SearchStrategy.ANCESTORS`**
    
    - Inspects only ancestor contexts (above), not the current one.
        
- **`SearchStrategy.CUSTOM`**
    
    - Allows defining a custom bean lookup strategy by implementing the `BeanFactorySearchStrategy` interface.
        

---

### Key Takeaways

1. **Conditional Concept**
    
    - `@ConditionalOnMissingBean` is part of Spring Boot’s conditional annotations that enable conditional bean registration based on the presence or absence of other beans.
        
2. **Default vs. Override**
    
    - By using this annotation, libraries and starters can supply sensible defaults while allowing applications to override them simply by defining their own beans.
        
3. **Main Attributes**
    
    - `value`/`type`: Specify classes to check.
        
    - `name`: Specify bean names to check for.
        
    - `search`: Control the scope of the lookup (context hierarchy).
        
4. **Common Use Cases**
    
    - Providing default `RestTemplate` or `WebClient`.
        
    - Supplying a default `ObjectMapper` (Jackson) or `CacheManager`.
        
    - Fallback `DataSource` when none is configured manually.
        
5. **Further Reading**
    
    - Delve into Spring Boot’s official documentation (see “Auto-Configuration” → “Conditional Annotations” chapter).
        
    - Review the Spring Boot source code, particularly `ConditionalOnMissingBeanCondition.java` in the `spring-boot-autoconfigure` module for implementation details.
