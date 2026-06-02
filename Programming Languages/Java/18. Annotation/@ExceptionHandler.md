### **`@ExceptionHandler`** **in Spring MVC**

#### **1. Definition**

`@ExceptionHandler` is an annotation provided by the Spring Framework for handling exceptions thrown during the execution of controller methods.

It is commonly used in Spring MVC and Spring Boot applications to define centralized or localized exception-handling logic. Instead of writing repetitive `try-catch` blocks inside every controller method, developers can use `@ExceptionHandler` to separate normal business logic from error-handling logic.

In simple terms:

```java
@ExceptionHandler
```

tells Spring:

“When this type of exception occurs, use this method to handle it.”

---

### **2. Basic Usage**

#### **2.1 Handling Exceptions inside a Controller**

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        if (id <= 0) {
            throw new IllegalArgumentException("User ID must be positive");
        }

        return new User(id, "Jasper");
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(
            IllegalArgumentException exception) {

        return ResponseEntity
                .badRequest()
                .body(exception.getMessage());
    }
}
```

In this example, if `IllegalArgumentException` is thrown inside `UserController`, Spring will automatically call the method annotated with `@ExceptionHandler`.

The response may look like:

```text
HTTP Status: 400 Bad Request
Body: User ID must be positive
```

---

### **3. Method Signature**

An `@ExceptionHandler` method can accept different types of parameters, such as:

```java
@ExceptionHandler(RuntimeException.class)
public ResponseEntity<String> handleRuntimeException(RuntimeException exception) {
    return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(exception.getMessage());
}
```

Common parameters include:

```java
Exception exception
HttpServletRequest request
HttpServletResponse response
WebRequest webRequest
```

For example:

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleException(
        Exception exception,
        HttpServletRequest request) {

    String path = request.getRequestURI();

    return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body("Error occurred at path: " + path);
}
```

---

### **4. Handling Multiple Exception Types**

One handler method can handle multiple exception classes.

```java
@ExceptionHandler({
        IllegalArgumentException.class,
        IllegalStateException.class
})
public ResponseEntity<String> handleBadRequest(RuntimeException exception) {
    return ResponseEntity
            .badRequest()
            .body(exception.getMessage());
}
```

This means that both `IllegalArgumentException` and `IllegalStateException` will be handled by the same method.

---

### **5. Returning a Custom Error Response**

In real projects, it is better to return a structured error response rather than a plain string.

#### **5.1 Error Response Class**

```java
public class ErrorResponse {

    private int code;
    private String message;
    private String path;

    public ErrorResponse(int code, String message, String path) {
        this.code = code;
        this.message = message;
        this.path = path;
    }

    public int getCode() {
        return code;
    }

    public String getMessage() {
        return message;
    }

    public String getPath() {
        return path;
    }
}
```

#### **5.2 Exception Handler**

```java
@ExceptionHandler(IllegalArgumentException.class)
public ResponseEntity<ErrorResponse> handleIllegalArgumentException(
        IllegalArgumentException exception,
        HttpServletRequest request) {

    ErrorResponse errorResponse = new ErrorResponse(
            400,
            exception.getMessage(),
            request.getRequestURI()
    );

    return ResponseEntity
            .badRequest()
            .body(errorResponse);
}
```

The response may look like:

```json
{
  "code": 400,
  "message": "User ID must be positive",
  "path": "/users/-1"
}
```

This style is more suitable for frontend-backend interaction because the frontend can parse the error response consistently.

---

### **6. Local Exception Handling**

When `@ExceptionHandler` is written inside a controller class, it only handles exceptions thrown by methods inside that controller.

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    @GetMapping("/{id}")
    public String getOrder(@PathVariable Long id) {
        if (id == null || id <= 0) {
            throw new IllegalArgumentException("Invalid order ID");
        }

        return "Order ID: " + id;
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgumentException(
            IllegalArgumentException exception) {

        return ResponseEntity
                .badRequest()
                .body(exception.getMessage());
    }
}
```

This handler only applies to `OrderController`.

---

### **7. Global Exception Handling with**

**`@ControllerAdvice`**

In real enterprise projects, `@ExceptionHandler` is usually combined with `@RestControllerAdvice` or `@ControllerAdvice`.

#### **7.1**

**`@RestControllerAdvice`**

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ErrorResponse> handleIllegalArgumentException(
            IllegalArgumentException exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                400,
                exception.getMessage(),
                request.getRequestURI()
        );

        return ResponseEntity
                .badRequest()
                .body(errorResponse);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(
            Exception exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                500,
                "Internal server error",
                request.getRequestURI()
        );

        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(errorResponse);
    }
}
```

`@RestControllerAdvice` can be understood as:

```java
@ControllerAdvice + @ResponseBody
```

Therefore, it is especially suitable for RESTful APIs because the returned object will be automatically converted into JSON.

---

### **8. Execution Priority**

When an exception is thrown, Spring tries to find the most specific matching exception handler.

For example:

```java
@ExceptionHandler(IllegalArgumentException.class)
public ResponseEntity<String> handleIllegalArgumentException(
        IllegalArgumentException exception) {
    return ResponseEntity.badRequest().body("Illegal argument");
}

@ExceptionHandler(RuntimeException.class)
public ResponseEntity<String> handleRuntimeException(
        RuntimeException exception) {
    return ResponseEntity.status(500).body("Runtime exception");
}

@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleException(
        Exception exception) {
    return ResponseEntity.status(500).body("General exception");
}
```

If the thrown exception is `IllegalArgumentException`, Spring will choose:

```java
handleIllegalArgumentException()
```

rather than:

```java
handleRuntimeException()
```

because `IllegalArgumentException` is more specific.

The inheritance relationship is:

```text
Exception
 └── RuntimeException
      └── IllegalArgumentException
```

Therefore, the more specific exception handler has higher priority.

---

### **9. Common Project Structure**

A common project structure is:

```text
src/main/java/com/example/demo
├── controller
│   └── UserController.java
├── exception
│   ├── BusinessException.java
│   └── GlobalExceptionHandler.java
├── response
│   └── ApiResponse.java
└── service
    └── UserService.java
```

Usually:

```text
BusinessException
```

is used to represent business-level errors, while:

```text
GlobalExceptionHandler
```

is used to handle all exceptions globally.

---

### **10. Custom Business Exception**

#### **10.1 Define a Business Exception**

```java
public class BusinessException extends RuntimeException {

    private final int code;

    public BusinessException(int code, String message) {
        super(message);
        this.code = code;
    }

    public int getCode() {
        return code;
    }
}
```

#### **10.2 Throw the Exception in Service Layer**

```java
@Service
public class UserService {

    public User getUserById(Long id) {
        if (id == null || id <= 0) {
            throw new BusinessException(400, "Invalid user ID");
        }

        // Simulate database query
        return new User(id, "Jasper");
    }
}
```

#### **10.3 Handle the Exception Globally**

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ErrorResponse> handleBusinessException(
            BusinessException exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                exception.getCode(),
                exception.getMessage(),
                request.getRequestURI()
        );

        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(errorResponse);
    }
}
```

This design makes business error handling clearer and easier to maintain.

---

### **11. Recommended Global Exception Handler Example**

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ErrorResponse> handleBusinessException(
            BusinessException exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                exception.getCode(),
                exception.getMessage(),
                request.getRequestURI()
        );

        return ResponseEntity
                .status(HttpStatus.BAD_REQUEST)
                .body(errorResponse);
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ErrorResponse> handleIllegalArgumentException(
            IllegalArgumentException exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                400,
                exception.getMessage(),
                request.getRequestURI()
        );

        return ResponseEntity
                .badRequest()
                .body(errorResponse);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(
            Exception exception,
            HttpServletRequest request) {

        ErrorResponse errorResponse = new ErrorResponse(
                500,
                "Internal server error",
                request.getRequestURI()
        );

        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(errorResponse);
    }
}
```

The final `Exception.class` handler is usually used as a fallback mechanism.

However, in production systems, it is not recommended to directly expose the original exception message to users for unknown system exceptions. Internal details should be logged on the server side, while the frontend should receive a safe and general error message.

---

### **12. Relationship with HTTP Status Codes**

`@ExceptionHandler` itself does not automatically decide the HTTP status code. The status code is usually controlled by one of the following approaches.

#### 

#### **12.1 Using**

**`ResponseEntity`**

```java
@ExceptionHandler(BusinessException.class)
public ResponseEntity<ErrorResponse> handleBusinessException(
        BusinessException exception,
        HttpServletRequest request) {

    return ResponseEntity
            .status(HttpStatus.BAD_REQUEST)
            .body(new ErrorResponse(
                    exception.getCode(),
                    exception.getMessage(),
                    request.getRequestURI()
            ));
}
```

#### **12.2 Using**

**`@ResponseStatus`**

```java
@ResponseStatus(HttpStatus.BAD_REQUEST)
@ExceptionHandler(IllegalArgumentException.class)
public ErrorResponse handleIllegalArgumentException(
        IllegalArgumentException exception,
        HttpServletRequest request) {

    return new ErrorResponse(
            400,
            exception.getMessage(),
            request.getRequestURI()
    );
}
```

In enterprise development, `ResponseEntity` is often more flexible because it allows developers to control both the response body and the HTTP status code dynamically.

---

### **13. Common Mistakes**

#### **13.1 Only Handling**

**`Exception.class`**

This is not recommended:

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleException(Exception exception) {
    return ResponseEntity.status(500).body(exception.getMessage());
}
```

The problem is that all exceptions are handled in the same way. Business errors, parameter errors, authentication errors, and system errors cannot be clearly distinguished.

A better approach is:

```java
@ExceptionHandler(BusinessException.class)
public ResponseEntity<ErrorResponse> handleBusinessException(...) {
    ...
}

@ExceptionHandler(IllegalArgumentException.class)
public ResponseEntity<ErrorResponse> handleIllegalArgumentException(...) {
    ...
}

@ExceptionHandler(Exception.class)
public ResponseEntity<ErrorResponse> handleException(...) {
    ...
}
```

---

#### **13.2 Exposing System Error Details**

This is unsafe:

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleException(Exception exception) {
    return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(exception.getMessage());
}
```

For example, it may expose database errors, SQL details, file paths, or internal class names.

A safer approach is:

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<ErrorResponse> handleException(
        Exception exception,
        HttpServletRequest request) {

    // Log the real exception internally
    exception.printStackTrace();

    ErrorResponse errorResponse = new ErrorResponse(
            500,
            "Internal server error",
            request.getRequestURI()
    );

    return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(errorResponse);
}
```

In real projects, logging should use a logging framework such as SLF4J instead of `printStackTrace()`.

---

### **14. Example with SLF4J Logging**

```java
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(
            Exception exception,
            HttpServletRequest request) {

        log.error("Unexpected exception occurred. path={}", 
                request.getRequestURI(), exception);

        ErrorResponse errorResponse = new ErrorResponse(
                500,
                "Internal server error",
                request.getRequestURI()
        );

        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(errorResponse);
    }
}
```

This approach keeps detailed error information in server logs while returning a clean response to the client.

---

### **15. Summary**

`@ExceptionHandler` is a Spring MVC annotation used to define methods for handling specific exceptions. It improves code maintainability by separating exception-handling logic from controller business logic.

In small applications, `@ExceptionHandler` can be placed directly inside a controller. In larger projects, it is usually combined with `@RestControllerAdvice` to build a global exception-handling mechanism.

A good exception-handling design should:

1. Distinguish business exceptions from system exceptions.
2. Return a unified response structure.
3. Use appropriate HTTP status codes.
4. Avoid exposing internal system details.
5. Log unexpected exceptions on the server side.
6. Keep controller methods focused on normal business logic.

A typical enterprise-level pattern is:

```text
Controller throws exception
        ↓
GlobalExceptionHandler catches exception
        ↓
Convert exception into unified error response
        ↓
Return JSON response to frontend
```

Therefore, `@ExceptionHandler` is not merely a syntax feature. It is an important part of building robust, maintainable, and standardized Spring-based backend systems.