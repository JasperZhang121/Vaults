`@RestController` is a specialized version of `@Controller` in Spring Framework, typically used in Spring Boot applications for RESTful web services. It is part of the Spring Web module and is used to define a controller class that <mark style="background: #FFB8EBA6;">handles HTTP requests and returns data in the form of JSON or XML</mark>, typically as the response body. Unlike `@Controller`, which generally returns views (HTML or JSP), `@RestController` automatically assumes that the return value of the methods should be serialized to HTTP response body.

### Key Features:

1. **Combines `@Controller` and `@ResponseBody`**:
    
    - `@RestController` is essentially a combination of `@Controller` and `@ResponseBody`. This means that all methods in a class annotated with `@RestController` will return data directly in the response body, without the need to annotate each method with `@ResponseBody`.
2. **JSON/XML Response**:
    
    - It automatically serializes the return object into JSON or XML based on the client request (via `Content-Type` header). Most often, JSON is returned for APIs, but XML is also supported if the client requests it.
3. **Convenient for REST APIs**:
    
    - This annotation is ideal for building REST APIs, as it simplifies the development process by automatically handling HTTP responses and removing the need for additional boilerplate code.

### Example Usage:

```java
@RestController
@RequestMapping("/api")
public class MyRestController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }

    @GetMapping("/user")
    public User getUser() {
        return new User("John", "Doe");
    }
}
```

### Example Explanation:

- **`@RestController`**: Marks the class as a REST controller that handles HTTP requests.
- **`@RequestMapping("/api")`**: The base URL for all methods in this controller. In this case, all API endpoints will start with `/api`.
- **`@GetMapping("/hello")`**: Handles GET requests to `/api/hello` and returns a simple `String`. This string will be returned in the response body.
- **`@GetMapping("/user")`**: Handles GET requests to `/api/user` and returns a `User` object, which will be automatically serialized to JSON and sent in the response body.

### Notes:

- `@RestController` is typically used in Spring Boot for REST APIs, where there is no need for returning views.
- If you need to customize response status codes or headers, you can use `@ResponseStatus` or manipulate the response using `ResponseEntity`.

### When to Use:

- If you're building a RESTful service with Spring Boot, and you need to return JSON or other raw data, `@RestController` is the preferred choice for simplicity and clarity.