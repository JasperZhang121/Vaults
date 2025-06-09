### Definition

`@FunctionalInterface` is a Java annotation introduced in Java 8, which indicates that an interface is intended to be a **functional interface**.

A **functional interface** is an interface that contains exactly **one abstract method** (also known as a Single Abstract Method or SAM interface). These interfaces can be implemented using **lambda expressions** or **method references**.

### Syntax

```java
@FunctionalInterface
public interface InterfaceName {
    void singleAbstractMethod();
}
```

### Characteristics of `@FunctionalInterface`:

- Must contain exactly **one abstract method**.
    
- Can include multiple **default methods** or **static methods**.
    
- Inherits methods from `java.lang.Object`, which do not count towards the abstract method count.
    
- Using the `@FunctionalInterface` annotation helps the compiler enforce the single abstract method rule.

### Example Usage:

Here's a simple example:

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}

public class FunctionalInterfaceExample {
    public static void main(String[] args) {
        // Using lambda expression to implement the interface
        Greeting greet = (name) -> System.out.println("Hello, " + name);
        greet.sayHello("Alice");
        
        // Output: Hello, Alice
    }
}
```


### Benefits of `@FunctionalInterface`:

- **Conciseness**: Allows implementation using lambda expressions, which makes code shorter and clearer.
    
- **Enforcement**: Ensures that the interface remains a valid functional interface.
    
- **Readability**: Makes intent explicit to developers reading your code.

### Built-in Functional Interfaces in Java:

Java provides several built-in functional interfaces in the `java.util.function` package, such as:

- `Predicate<T>` - evaluates a condition and returns a boolean.
    
- `Function<T, R>` - takes an input and returns an output.
    
- `Consumer<T>` - consumes an input and returns nothing (void).
    
- `Supplier<T>` - provides a result without taking any input.
    

Example with `Predicate`:

```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<Integer> isEven = (number) -> number % 2 == 0;

        System.out.println(isEven.test(4)); // true
        System.out.println(isEven.test(5)); // false
    }
}
```
