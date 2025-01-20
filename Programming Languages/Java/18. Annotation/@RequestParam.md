`@RequestParam` is a Spring MVC annotation used to extract query parameters, form parameters, or path parameters from the HTTP request in a controller method. It's commonly used in web applications built with Spring Framework to access data sent by the client as part of the URL or request body.
### 1. Query Parameters

If a client sends data as part of the URL query string, you can use `@RequestParam` to retrieve those values in your controller method.

#### Example:

```java
@GetMapping("/greet")
public String greet(@RequestParam String name) {
    return "Hello, " + name;
}
```

**URL Example:**

```
http://localhost:8080/greet?name=John
```

In this case, the value of the query parameter `name` is passed to the `greet` method.

### 2. Optional Parameters

You can specify default values or make parameters optional.

#### Example:

```java
@GetMapping("/greet")
public String greet(@RequestParam(defaultValue = "Guest") String name) {
    return "Hello, " + name;
}
```

In this case, if the `name` parameter is missing in the request, the default value `"Guest"` will be used.

### 3. Multiple Parameters

You can retrieve multiple parameters in the same method.

#### Example:

```java
@GetMapping("/greet")
public String greet(@RequestParam String name, @RequestParam int age) {
    return "Hello, " + name + ". You are " + age + " years old.";
}
```

**URL Example:**

```
http://localhost:8080/greet?name=John&age=30
```

### 4. Using `@RequestParam` with `Map` or `MultiValueMap`

You can also retrieve all parameters in a `Map` or `MultiValueMap`.

#### Example:

```java
@GetMapping("/greet")
public String greet(@RequestParam Map<String, String> params) {
    return "Parameters: " + params;
}
```

This allows you to grab all the query parameters in a `Map`.

### 5. Path Parameters with `@PathVariable`

If you're dealing with path parameters (like `/user/{id}`), you would typically use `@PathVariable` instead of `@RequestParam`.

#### Example:

```java
@GetMapping("/user/{id}")
public String getUserById(@PathVariable String id) {
    return "User ID: " + id;
}
```

### 6. Binding to JavaBeans

You can also bind request parameters to a JavaBean using `@RequestParam`. In this case, Spring will match request parameters to bean properties.

#### Example:

```java
public class User {
    private String name;
    private int age;

    // Getters and setters
}

@GetMapping("/greet")
public String greet(@RequestParam User user) {
    return "Hello, " + user.getName() + ". You are " + user.getAge() + " years old.";
}
```

In this case, Spring will try to bind the `name` and `age` parameters to the `User` object's fields.

### Summary of `@RequestParam`

- **Used for query parameters or form data**: `@RequestParam` binds parameters from the URL or request body to method arguments.
- **Default values**: You can specify default values if the parameter is missing.
- **Optional**: By setting `required = false`, parameters become optional.
- **Mapping multiple parameters**: You can extract all parameters using `Map` or `MultiValueMap`.

It’s a flexible way of working with HTTP requests in Spring MVC.