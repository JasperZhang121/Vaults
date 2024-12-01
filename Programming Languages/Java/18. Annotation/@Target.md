`@Target` is an annotation used in Java to specify where an annotation can be applied. It restricts the possible locations for the annotation in the code.

Here’s how `@Target` works:

### Syntax

```java
@Target(ElementType.LOCATION)
```

- `ElementType.LOCATION` can be any of the following values, depending on where you want to apply the annotation:
    - `FIELD`: Can be applied to fields (including member variables).
    - `METHOD`: Can be applied to methods.
    - `CONSTRUCTOR`: Can be applied to constructors.
    - `PARAMETER`: Can be applied to method parameters.
    - `TYPE`: Can be applied to classes, interfaces (including annotations), or enums.
    - `LOCAL_VARIABLE`: Can be applied to local variables.
    - `ANNOTATION_TYPE`: Can be applied to other annotations.
    - `PACKAGE`: Can be applied to a package.
    - `TYPE_PARAMETER`: Can be applied to type parameters (Java 8+).
    - `TYPE_USE`: Can be applied to any type (Java 8+).

### Example:

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)  // The annotation can only be applied to methods
public @interface MyMethodAnnotation {
    String value() default "Default Value";
}

public class Example {

    @MyMethodAnnotation(value = "Hello")
    public void myMethod() {
        System.out.println("Method with annotation.");
    }
}
```

In this example, `@MyMethodAnnotation` can only be applied to methods because of the `@Target(ElementType.METHOD)` annotation. If you try to use it elsewhere, like on a field, the code will not compile.

### Key Points:

- `@Target` restricts where the annotation can be applied.
- It is often used with `@Retention`, which determines whether the annotation is available at runtime or compile-time.
- Using `@Target` helps enforce good practices by preventing annotations from being misapplied to incorrect elements in the code.