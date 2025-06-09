The `@ConfigurationProperties` annotation in Spring Framework is used to map properties defined in configuration files (like `application.properties` or `application.yml`) into a Java class. This allows you to easily manage and access configuration properties in a type-safe and structured way.

### Key Features:

1. **Type-Safe Configuration**: Maps properties to fields in a Java object, ensuring type safety.
2. **Hierarchical Property Binding**: Supports hierarchical binding of properties, making it easy to group related properties.
3. **Validation Support**: Works well with bean validation annotations like `@NotNull`, `@Min`, etc., to validate the properties.
4. **Default Values**: You can provide default values directly in the Java class fields.

### Example Usage:

#### 1. Define Configuration Class

```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConfigurationProperties(prefix = "app")
public class AppProperties {

    private String name;
    private int version;

    // Getters and Setters
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getVersion() {
        return version;
    }

    public void setVersion(int version) {
        this.version = version;
    }
}
```

#### 2. Configuration File (`application.yml`)

```yaml
app:
  name: MyApplication
  version: 1
```

#### 3. Using the Properties

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AppController {

    @Autowired
    private AppProperties appProperties;

    @GetMapping("/info")
    public String getAppInfo() {
        return "App Name: " + appProperties.getName() + ", Version: " + appProperties.getVersion();
    }
}
```

### Key Points to Remember:

- **Prefix**: The `prefix` attribute in `@ConfigurationProperties` specifies which properties to bind. In the example above, all properties starting with `app` are mapped.
- **Enable Binding**: Ensure you have the `@EnableConfigurationProperties` annotation on a configuration class or register the properties class as a bean manually if not annotated with `@Configuration`.
- **Validation**: Add validation annotations (like `@NotNull`) to fields for property validation.

#### Validation Example:

```java
import javax.validation.constraints.NotNull;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConfigurationProperties(prefix = "app")
public class AppProperties {

    @NotNull
    private String name;

    private int version;

    // Getters and Setters
}
```

If the `name` property is not present in the configuration file, the application will fail to start with a validation error.
