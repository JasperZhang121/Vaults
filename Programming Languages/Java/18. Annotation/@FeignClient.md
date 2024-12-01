
The `@FeignClient` annotation is used in **Spring Cloud** to declare a client interface for making HTTP requests to other services. Feign simplifies the process of interacting with RESTful APIs by allowing you to define methods and map them to specific endpoints, removing the need to manually use `RestTemplate` or other HTTP libraries.

#### Key Features:

- Helps build HTTP clients declaratively.
- Supports load balancing when combined with Ribbon or Eureka.
- Automatically integrates with Spring Cloud for service discovery and fault tolerance (when combined with Resilience4j/Hystrix).

#### When to Use:

Use `@FeignClient` when your application needs to make RESTful API calls to external services or microservices. It is commonly used in a microservices architecture to facilitate communication between services.

#### Example:

Suppose you have a service `OrderService` that needs to fetch user information from a `UserService`:

##### Step 1: Add Feign Dependency (in `pom.xml`)

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

##### Step 2: Enable Feign Clients in your Spring Boot application

```java
@SpringBootApplication
@EnableFeignClients
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}
```

##### Step 3: Declare the `@FeignClient` interface

```java
@FeignClient(name = "user-service", url = "http://localhost:8081")
public interface UserServiceClient {

    @GetMapping("/users/{id}")
    User getUserById(@PathVariable("id") Long id);
}
```

- `name`: Logical name of the client (used for load balancing or service discovery).
- `url`: Base URL of the target service (can be omitted if using service discovery).

##### Step 4: Use the Feign client in your code

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    private final UserServiceClient userServiceClient;

    public OrderController(UserServiceClient userServiceClient) {
        this.userServiceClient = userServiceClient;
    }

    @GetMapping("/{orderId}")
    public OrderResponse getOrder(@PathVariable Long orderId) {
        // Mock order data
        Order order = new Order(orderId, 123L, "Laptop");

        // Fetch user details from UserService
        User user = userServiceClient.getUserById(order.getUserId());

        // Combine order and user data
        return new OrderResponse(order, user);
    }
}
```

##### Supporting Classes

```java
public class User {
    private Long id;
    private String name;
    private String email;
    // Getters and setters...
}

public class Order {
    private Long id;
    private Long userId;
    private String itemName;
    // Getters and setters...
}

public class OrderResponse {
    private Order order;
    private User user;
    // Constructor, getters, setters...
}
```
