`@Documented` is a marker annotation in Java that is used to indicate that the annotation it is applied to should be documented by the Javadoc tool. If an annotation is marked with `@Documented`, it will be included in the generated documentation, even though the annotation itself does not affect the code execution.

Example of how it's used:

```java
import java.lang.annotation.Documented;

@Documented
public @interface MyAnnotation {
    String value() default "default";
}
```

### Purpose:

The primary purpose of `@Documented` is to include custom annotations in the Javadoc documentation. Without it, annotations are not included in Javadoc output unless explicitly documented within the annotation itself.

### When to Use:

You should use `@Documented` when you want the annotation to appear in the Javadoc for your API. This is particularly useful for annotations that serve as metadata for code and are intended to be used in public APIs.