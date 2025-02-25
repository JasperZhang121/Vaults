The @Primary annotation in Spring is used to mark a bean as the default choice when there are multiple candidates of the same type in the Spring application context. This is particularly useful when Spring encounters ambiguity while autowiring beans—when multiple beans of the same type are present and Spring doesn’t know which one to inject.

When you mark a bean with @Primary, Spring will automatically choose that bean when it encounters an autowiring conflict, meaning it will inject the bean annotated with @Primary by default unless explicitly specified otherwise using @Qualifier.

How It Works:
	•	Ambiguity Resolution: If you have multiple beans of the same type in the Spring context, Spring will throw an exception because it won’t know which bean to inject. To resolve this, you can use @Primary on one of the beans to indicate that it should be the default choice when autowiring.
	•	Usage with @Autowired: The @Primary annotation works in combination with @Autowired. If there are multiple beans of the same type, the bean annotated with @Primary is selected for injection.

Example:

```java
@Configuration
public class AppConfig {

    @Bean
    @Primary
    public Service serviceA() {
        return new ServiceA();
    }

    @Bean
    public Service serviceB() {
        return new ServiceB();
    }
}

@Service
public class MyService {

    private final Service service;

    @Autowired
    public MyService(Service service) {
        this.service = service; // Spring will inject serviceA by default
    }
}
```


In this example:
	•	serviceA is marked with @Primary, so it will be injected into MyService by default when Spring autowires the Service type.
	•	Even though there are two beans (serviceA and serviceB), serviceA will be selected because it is marked as the primary one.

When to Use @Primary:
	•	Resolve Autowiring Ambiguity: Use @Primary when you have multiple beans of the same type and want to specify one as the default to be injected.
	•	Fine-Tune Bean Injection: It’s helpful when you want to inject a specific bean by default but still provide other options for special cases.

Example with @Qualifier:

You can still use @Qualifier to specify which bean you want to inject if you don’t want to rely on the default choice (i.e., the one marked with @Primary).

```java
@Configuration
public class AppConfig {

    @Bean
    @Primary
    public Service serviceA() {
        return new ServiceA();
    }

    @Bean
    public Service serviceB() {
        return new ServiceB();
    }
}

@Service
public class MyService {

    private final Service service;

    @Autowired
    @Qualifier("serviceB") // This will inject serviceB instead of serviceA, even though serviceA is primary
    public MyService(Service service) {
        this.service = service;
    }
}
```

Here, even though serviceA is marked as @Primary, @Qualifier is used to inject serviceB explicitly.

Key Points:
	1.	Default Bean Selection: When multiple beans of the same type exist, @Primary marks one as the default.
	2.	Works with @Autowired: Spring will automatically inject the primary bean if no other specific bean is mentioned.
	3.	Use with @Qualifier for Explicit Selection: You can override the default choice by using @Qualifier to specify which bean to inject when necessary.