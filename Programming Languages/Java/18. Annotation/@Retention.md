
The `@Retention` annotation in Java is used to specify how long annotations should be retained and where they are available. It is defined in the `java.lang.annotation` package and works in conjunction with the `@Target` annotation to describe how an annotation behaves during the lifecycle of the program.

### `@Retention` Annotation

The `@Retention` annotation takes a single element, which is one of the following retention policies:

1. **`RetentionPolicy.SOURCE`**:
    
    - The annotation is discarded by the compiler and is not available in the bytecode. It is typically used for annotations that are only needed during compile-time (e.g., for code generation, checks, or other compile-time operations).
    - Example usage:
        
        ```java
        @Retention(RetentionPolicy.SOURCE)
        public @interface MySourceOnlyAnnotation {}
        ```
        
2. **`RetentionPolicy.CLASS`**:
    
    - The annotation is retained in the bytecode but is not available at runtime. This is the default retention policy if `@Retention` is not specified.
    - Example usage:
        
        ```java
        @Retention(RetentionPolicy.CLASS)
        public @interface MyClassOnlyAnnotation {}
        ```
        
3. **`RetentionPolicy.RUNTIME`**:
    
    - The annotation is retained at runtime and can be accessed via reflection (i.e., through `java.lang.reflect` classes like `Method` or `Field`).
    - This is useful for annotations that are needed at runtime for frameworks like Spring, Hibernate, etc.
    - Example usage:
        
        ```java
        @Retention(RetentionPolicy.RUNTIME)
        public @interface MyRuntimeAnnotation {}
        ```
        

### Example:

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyCustomAnnotation {
    String value() default "Default Value";
}

public class Example {
    @MyCustomAnnotation(value = "Test Annotation")
    public void myMethod() {
        System.out.println("Method with custom annotation executed.");
    }

    public static void main(String[] args) throws Exception {
        Example example = new Example();
        example.myMethod();

        // Accessing annotation at runtime via reflection
        MyCustomAnnotation annotation = example.getClass().getMethod("myMethod").getAnnotation(MyCustomAnnotation.class);
        System.out.println("Annotation Value: " + annotation.value());
    }
}
```

In this example:

- The `MyCustomAnnotation` annotation is retained at runtime (`@Retention(RetentionPolicy.RUNTIME)`).
- The annotation is applied to the `myMethod` method.
- The annotation is accessed at runtime via reflection, and its `value()` element is printed out.

### Summary:

- **`@Retention(RetentionPolicy.SOURCE)`**: Annotations available only at compile-time.
- **`@Retention(RetentionPolicy.CLASS)`**: Annotations retained in bytecode but not available at runtime (default).
- **`@Retention(RetentionPolicy.RUNTIME)`**: Annotations available at runtime and accessible through reflection.
