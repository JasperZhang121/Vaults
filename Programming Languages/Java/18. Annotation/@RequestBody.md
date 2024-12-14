`@RequestBody` is a Spring MVC annotation used to map the **HTTP request body** to a Java object. <mark style="background: #FFB8EBA6;">It allows you to read data sent by the client (usually in JSON or XML format) and convert it into a Java object</mark>, making it easy to handle input data in RESTful APIs.

### Key Features:

1. **Request Body to Java Object**:
    
    - Spring uses **HttpMessageConverter** to convert the request body into a Java object.
    - Typically, JSON data sent by the client is deserialized into a Java object using the Jackson library (default in Spring Boot).
2. **Annotation Placement**:
    
    - Used as a parameter annotation in controller methods.
3. **Common Use Cases**:
    
    - Receiving JSON objects from the client in POST, PUT, or DELETE requests.
    - Automatically converting the received JSON/XML payload into Java objects.

### Example:

#### JSON Request Body:

The client sends the following JSON payload:

```json
{
    "name": "John Doe",
    "age": 30
}
```

#### Java Class to Map Request:

```java
public class User {
    private String name;
    private int age;

    // Getters and setters
}
```

#### Controller Using `@RequestBody`:

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping("/create")
    public String createUser(@RequestBody User user) {
        return "User created: " + user.getName() + ", Age: " + user.getAge();
    }
}
```

When the client sends the above JSON payload in a POST request to `/users/create`, the `@RequestBody` annotation will:

1. Automatically map the JSON fields to the corresponding fields in the `User` object.
2. Pass the populated `User` object to the `createUser` method.

#### Response:

```
User created: John Doe, Age: 30
```

### Customizing Behavior:

1. **Validation with `@Valid`**: Combine `@RequestBody` with `@Valid` to validate the input.
    
    Example:
    
    ```java
    @PostMapping("/create")
    public String createUser(@Valid @RequestBody User user) {
        return "User created: " + user.getName();
    }
    ```
    
2. **Handling Optional Fields**: If some fields are optional, you can use wrapper classes like `Optional` or make them nullable.
    
    Example:
    
    ```java
    public class User {
        private String name;
        private Integer age; // Nullable
        // Getters and setters
    }
    ```
    
3. **Custom Deserialization**: You can customize how the JSON is deserialized by adding annotations like `@JsonProperty` or `@JsonNaming` in the Java object.

### When to Use `@RequestBody`:

- When your REST endpoint expects a JSON (or XML) payload in the request body.
- For POST and PUT operations that require structured input data.
- When you need to automatically convert request data to Java objects.

### Common Issues:

1. **Unsupported Media Type (415)**:
    
    - Occurs if the `Content-Type` header is not set to `application/json` or the server cannot parse the request body format.
    
    Solution: Ensure the client sends data with the correct `Content-Type` header.
    
2. **Null Values in Java Object**:
    
    - Happens when the JSON fields don't match the Java object's field names.
    
    Solution: Ensure the JSON field names match the Java object's fields or use annotations like `@JsonProperty`.
