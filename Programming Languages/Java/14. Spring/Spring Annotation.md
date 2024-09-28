Spring Boot simplifies the development of Spring applications by providing a range of annotations that help automate configuration and reduce boilerplate code. Here's an overview of some commonly used annotations in Spring Boot, categorized by their typical use cases:

### Core Spring Framework Annotations

1. **@SpringBootApplication**
   - Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan` annotations. It's used on the main class to enable auto-configuration and component scanning.

   ```java
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class MySpringBootApplication {

      public static void main(String[] args) {
         SpringApplication.run(MySpringBootApplication.class, args);
      }
   }
   ```

2. **@Autowired** 
   - Allows for automatic dependency injection. It can be used on fields, setters, or constructors to inject beans automatically.


- Field Injection
   ```java
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Component;

   @Component
   public class VehicleService {

      @Autowired
      private Engine engine;

      public void startEngine() {
         engine.start();
      }
   }
   ```

- Constructor Injection

   ```java
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Component;

   @Component
   public class VehicleService {

      private final Engine engine;

      @Autowired
      public VehicleService(Engine engine) {
         this.engine = engine;
      }

      public void startEngine() {
         engine.start();
      }
   }
   ```

- General

   ```java
   package com.example.demo;

   public class Engine {
      private String maker;
      private int madeYear;

      public Engine(String maker, int madeYear) {
         this.maker = maker;
         this.madeYear = madeYear;
      }

      public void start() {
         System.out.println("Engine started by " + maker + " in " + madeYear);
      }

      public void stop() {
         System.out.println("Engine stopped.");
      }

      // Getters and Setters
      public String getMaker() {
         return maker;
      }

      public void setMaker(String maker) {
         this.maker = maker;
      }

      public int getMadeYear() {
         return madeYear;
      }

      public void setMadeYear(int madeYear) {
         this.madeYear = madeYear;
      }
   }
   ```

   ```java
   package com.example.demo;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Component;

   @Component
   public class Vehicle {
      @Autowired
      private Engine engine;

      public void startEngine() {
         engine.start();
      }

      public void stopEngine() {
         engine.stop();
      }
   }
   ```

   ```java
   package com.example.demo;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;

   @Configuration
   public class AppConfig {
      @Bean
      public Engine engine() {
         return new Engine("Honda", 2020);
      }

      @Bean
      public Vehicle vehicle() {
         return new Vehicle();
      }
   }
```

3. **@Component**
   - Indicates that a class is a Spring component. Classes with this annotation are automatically detected through classpath scanning.

- Basic Usage
   ```java
   package com.example.app.components;
   import org.springframework.stereotype.Component;

   @Component
   public class VehicleService {
      public String getServiceType() {
         return "Regular service";
      }
   }
   ```

- Dependency Injection

   ```java
   package com.example.app.components;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Component;

   @Component
   public class VehicleController {

      private final VehicleService vehicleService;

      @Autowired
      public VehicleController(VehicleService vehicleService) {
         this.vehicleService = vehicleService;
      }

      public void performService() {
         System.out.println("Service performed: " + vehicleService.getServiceType());
      }
   }
   ```

- Named Component

   ```java
   package com.example.app.components;

   import org.springframework.stereotype.Component;

   @Component("customVehicleService")
   public class CustomVehicleService {
      public String getServiceDetails() {
         return "Custom vehicle service details";
      }
   }
   ```

- Web Application

   ```java
   import org.springframework.stereotype.Component;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   @Component
   public class WelcomeController {

      @GetMapping("/")
      public String welcome() {
         return "Welcome to Spring App!";
      }
   }
   ```

4. **@Service, @Repository, @Controller**
   - Specializations of `@Component` that indicate the role of the component within the application (service layer, data access layer, and presentation layer, respectively).
   - The @Service annotation is used in the service layer of an application. This layer typically involves business logic, operations on data sent to and from the DAO layer, and calls to external APIs. This annotation indicates that a class holds business logic.
   - The @Repository annotation is used in the data access layer, which directly deals with the data source. It abstracts and encapsulates the data access logic and translates exceptions from the data access technology used (like JPA, Hibernate) into Spring's consistent exception hierarchy.
   - The @Controller annotation marks a class as a web controller, which can handle HTTP requests. This layer is typically where you define your application's endpoints, handling web requests and responses.

- @Service
   ```java
   package com.example.app.services;

   import org.springframework.stereotype.Service;
   import com.example.app.models.Account;
   import com.example.app.repositories.AccountRepository;
   import org.springframework.beans.factory.annotation.Autowired;

   @Service
   public class AccountService {

      private final AccountRepository accountRepository;

      @Autowired
      public AccountService(AccountRepository accountRepository) {
         this.accountRepository = accountRepository;
      }

      public Account getAccount(Long accountId) {
         return accountRepository.findById(accountId).orElse(null);
      }

      public Account updateAccountBalance(Long accountId, Double newBalance) {
         Account account = accountRepository.findById(accountId).orElseThrow(() -> new IllegalArgumentException("Invalid account ID"));
         account.setBalance(newBalance);
         return accountRepository.save(account);
      }
   }
   ```

- @Repository

   ```java
   package com.example.app.repositories;

   import org.springframework.data.jpa.repository.JpaRepository;
   import org.springframework.stereotype.Repository;
   import com.example.app.models.Account;

   @Repository
   public interface AccountRepository extends JpaRepository<Account, Long> {
   }
   ```

- Controller
   ```java
   package com.example.app.controllers;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.app.services.AccountService;

   @RestController
   public class AccountController {

      private final AccountService accountService;

      @Autowired
      public AccountController(AccountService accountService) {
         this.accountService = accountService;
      }

      @GetMapping("/accounts/{id}")
      public Account getAccount(@PathVariable Long id) {
         return accountService.getAccount(id);
      }
   }
   ```

5. **@Configuration**
   - Indicates that a class declares one or more `@Bean` methods and may be processed by the Spring container to generate bean definitions and service requests for those beans at runtime.

- Basic Configuration Class

   ```java
   package com.example.config;

   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import com.example.services.MyService;
   import com.example.repositories.MyRepository;

   @Configuration
   public class AppConfig {

      @Bean
      public MyService myService() {
         return new MyService(myRepository());
      }

      @Bean
      public MyRepository myRepository() {
         return new MyRepository();
      }
   }
   ```

- Configuration with External Properties
   ```java
   package com.example.config;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.PropertySource;
   import com.example.services.ExternalService;

   @Configuration
   @PropertySource("classpath:config.properties")
   public class ExternalConfig {

      @Value("${service.url}")
      private String serviceUrl;

      @Bean
      public ExternalService externalService() {
         return new ExternalService(serviceUrl);
      }
   }
   ```
   Explanation:

   - ExternalConfig includes @PropertySource to specify the location of a properties file.
   - @Value is used to inject the value of service.url from the properties file into the serviceUrl field.
   - externalService() creates an instance of ExternalService with a property injected from the configuration.

- Importing Additional Configurations

   ```java
   package com.example.config;

   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Import;
   import com.example.services.AnotherService;

   @Configuration
   @Import(DatabaseConfig.class)
   public class MainConfig {

      @Bean
      public AnotherService anotherService() {
         return new AnotherService();
      }
   }
   ```
   Explanation:

   - MainConfig uses @Import to include another configuration class (DatabaseConfig). This is useful for organizing different parts of your configuration into modular classes.
   - AnotherService bean is defined in MainConfig, which may depend on beans defined in DatabaseConfig.

- Conditional Beans

   ```java
   package com.example.conditions;

   import org.springframework.context.annotation.Condition;
   import org.springframework.context.annotation.ConditionContext;
   import org.springframework.core.type.AnnotatedTypeMetadata;

   public class OnProductionCondition implements Condition {
      @Override
      public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
         String environment = context.getEnvironment().getProperty("env");
         return "production".equals(environment);
      }
   }
   ```
   ```java
   package com.example.config;

   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Conditional;
   import com.example.conditions.OnProductionCondition;
   import com.example.services.ProductionService;

   @Configuration
   public class ConditionalConfig {

      @Bean
      @Conditional(OnProductionCondition.class)
      public ProductionService productionService() {
         return new ProductionService();
      }
   }
   ```

   Explanation:
   - ConditionalConfig defines a bean method with the @Conditional annotation, which specifies that the productionService() bean should only be created if the specified condition (OnProductionCondition.class) is met.
   - This approach is useful for defining beans that should only be available in certain environments (e.g., production vs. development).

- Profile-specific Configuration
   ```java
   package com.example.config;

   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Profile;
   import com.example.services.DevelopmentService;

   @Configuration
   @Profile("dev")
   public class DevConfig {

      @Bean
      public DevelopmentService developmentService() {
         return new DevelopmentService();
      }
   }
   ```

   - DevConfig is marked with @Profile("dev"), which limits the configuration to only be active when the "dev" profile is active.
   - This is particularly useful for defining beans that should only be configured during development, such as mock implementations or development-only services.

6. **@Bean**
   - Used on methods within a `@Configuration` annotated class to define a bean.

   ```java
   package com.example.config;

   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.context.annotation.Profile;
   import com.example.services.UserService;
   import com.example.repositories.UserRepository;
   import com.example.services.MessagingService;
   import com.example.services.DevelopmentService;
   import com.example.utils.StringUtil;

   @Configuration
   public class AppConfig {

      // Example 1: Basic Bean Definition
      @Bean
      public UserService userService() {
         return new UserService();
      }

      // Example 2: Bean with Dependencies
      @Bean
      public UserRepository userRepository() {
         return new UserRepository();
      }

      @Bean
      public UserService userService(UserRepository userRepository) {
         return new UserService(userRepository);
      }

      // Example 3: Bean with Initialization and Destruction Methods
      @Bean(initMethod = "init", destroyMethod = "cleanup")
      public MessagingService messagingService() {
         return new MessagingService();
      }

      // Example 4: Profile-specific Bean Configuration
      @Configuration
      @Profile("dev")
      static class DevConfig {
         @Bean
         public DevelopmentService developmentService() {
               return new DevelopmentService();
         }
      }

      // Example 5: Bean with Factory Method
      @Bean
      public StringUtil stringUtil() {
         return StringUtil.getInstance();
      }
   }
   ```


### Web Development Annotations

1. **@RestController**
   - A specialization of `@Controller`. It indicates that the class handles requests in a RESTful web service. `@RestController` combines `@Controller` and `@ResponseBody`.

- Basic REST Controller

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class SimpleController {

      @GetMapping("/hello")
      public String sayHello() {
         return "Hello, World!";
      }
   }
   ```

   **Explanation:**
   - `@RestController` makes this class a controller where every method returns a domain object instead of a view. It simplifies the controller implementation by eliminating the need to annotate every response body with `@ResponseBody`.
   - `@GetMapping("/hello")` maps HTTP GET requests to the `sayHello()` method, which returns a simple greeting as a response.

- REST Controller with Path Variable

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class UserController {

      @GetMapping("/users/{userId}")
      public String getUserById(@PathVariable String userId) {
         return "User ID: " + userId;
      }
   }
   ```

   **Explanation:**
   - `@GetMapping("/users/{userId}")` maps HTTP GET requests for `/users/{userId}` to the `getUserById` method.
   - `@PathVariable` is used to extract `userId` from the URL and pass it as a parameter to the method.

- REST Controller Returning an Object
   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.models.User;

   @RestController
   public class UserProfileController {

      @GetMapping("/profile")
      public User getUserProfile() {
         return new User("JohnDoe", "John", "Doe");
      }
   }
   ```

   **Explanation:**
   - The `getUserProfile` method returns a `User` object with predefined data.
   - Spring automatically serializes the `User` object into JSON (or XML, depending on the configuration and request headers) due to the `@RestController` annotation.

- REST Controller with Request Parameters
   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class GreetingController {

      @GetMapping("/greet")
      public String greet(@RequestParam(name = "name", defaultValue = "World") String name) {
         return "Hello, " + name + "!";
      }
   }
   ```

   **Explanation:**
   - `@RequestParam` allows the method to accept a query parameter from the URL, optionally specifying a default value if the parameter is not provided.
   - The method constructs a greeting message using the provided name or the default value.

- REST Controller with Post Mapping
   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.models.User;

   @RestController
   public class UserRegistrationController {

      @PostMapping("/register")
      public User registerUser(@RequestBody User user) {
         // Here you would typically add the user to a database
         return user;  // Just returning the user for demonstration purposes
      }
   }
   ```
   **Explanation:**
   - `@PostMapping("/register")` maps HTTP POST requests to the `registerUser` method.
   - `@RequestBody` annotation is used to bind the HTTP request body with an object in the method parameter, indicating that a method parameter should be bound to the body of the web request.


2. **@RequestMapping (and variations like @GetMapping, @PostMapping, etc.)**

   - Maps HTTP requests to handler methods of MVC and REST controllers. The specific variations like `@GetMapping` and `@PostMapping` are shortcuts for common request types.

- Basic Usage of `@RequestMapping`

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class GeneralController {

      @RequestMapping(value = "/home", method = RequestMethod.GET)
      public String homePage() {
         return "Welcome to the Home Page!";
      }

      @RequestMapping(value = "/home", method = RequestMethod.POST)
      public String submitHomeData() {
         return "Data submitted to Home Page!";
      }
   }
   ```
   **Explanation:**
   - `@RequestMapping(value = "/home", method = RequestMethod.GET)` maps GET requests to `homePage()`.
   - `@RequestMapping(value = "/home", method = RequestMethod.POST)` maps POST requests to `submitHomeData()`.

- Using `@GetMapping`

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class UserController {

      @GetMapping("/users")
      public String listUsers() {
         return "Listing all users";
      }
   }
   ```

   **Explanation:**
   - `@GetMapping("/users")` is a shortcut for `@RequestMapping(method = RequestMethod.GET)`, specifically for handling GET requests. It maps GET requests to `listUsers()`.

- Using `@PostMapping`

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.models.User;

   @RestController
   public class RegistrationController {

      @PostMapping("/register")
      public String registerUser(@RequestBody User user) {
         // logic to handle user registration
         return "User registered successfully!";
      }
   }
   ```

   **Explanation:**
   - `@PostMapping("/register")` maps POST requests to the `registerUser()` method.
   - `@RequestBody` indicates that the parameter should be bound to the body of the web request.

- Using `@PutMapping`

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.PutMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import com.example.models.User;

   @RestController
   @RequestMapping("/users")
   public class UpdateController {

      @PutMapping("/{userId}")
      public String updateUser(@RequestBody User user) {
         // logic to update a user
         return "User updated successfully!";
      }
   }
   ```

   **Explanation:**
   - `@PutMapping("/{userId}")` maps PUT requests to the `updateUser()` method.
   - It's scoped under the `/users` path due to the class-level `@RequestMapping("/users")`.

- Using `@DeleteMapping`

   ```java
   package com.example.web;

   import org.springframework.web.bind.annotation.DeleteMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RestController;

   @RestController
   public class DeleteUserController {

      @DeleteMapping("/users/{userId}")
      public String deleteUser(@PathVariable String userId) {
         // logic to delete a user
         return "User deleted successfully!";
      }
   }
   ```

   **Explanation:**
   - `@DeleteMapping("/users/{userId}")` maps DELETE requests to the `deleteUser()` method.
   - `@PathVariable` extracts the `userId` from the URI path.

3. **@ResponseBody**
   - Indicates that the return value of a method should be used as the response body of the request.

4. **@PathVariable**
   - Used to extract values from the URI. Typically used in RESTful web services.

5. **@RequestParam**
   - Binds request parameters to a method parameter in your controller.

### Data Access Annotations

1. **@Entity**
   - Part of JPA (Java Persistence API). It indicates that a class is a JPA entity.
  
2. **@Table**
   - Specifies the primary table for the annotated entity in JPA.

3. **@Id**
   - Specifies the primary key of an entity in JPA.

4. **@Column**
   - Specifies a column in the database table corresponding to a persistent property or field.

5. **@Transactional**
   - Describes a transaction attribute on an individual method or on a class.

### Configuration and Properties

1. **@Value**
   - Used to inject property values into components.

2. **@PropertySource**
   - Used to declare a set of properties (defined in a property file) that Spring will load.

3. **@Profile**
   - Indicates that a component is eligible for registration when one or more specified profiles are active.

### Aspect-Oriented Programming (AOP)

1. **@Aspect**
   - Marks a class as an aspect.

2. **@Before, @After, @Around, @AfterReturning, @AfterThrowing**
   - Used to declare before, after, and around advice for aspect-oriented programming.

### Testing

1. **@SpringBootTest**
   - Used to bootstrap the full application context in a test environment.

2. **@MockBean**
   - Used in Spring Boot tests to add mock objects to the Spring application context.

3. **@DataJpaTest, @WebMvcTest, @RestClientTest**
   - Used for specific testing configurations tailored to JPA, MVC, and REST client environments, respectively.

