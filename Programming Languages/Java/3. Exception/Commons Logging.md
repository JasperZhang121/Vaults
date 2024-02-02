
Commons Logging, a third-party logging library, is different from the logging functionality provided in the Java standard library. Created by Apache, Commons Logging's main feature is its ability to plug into different logging systems. By default, it automatically searches for and uses Log4j (another popular logging system); if Log4j is not found, it falls back to using JDK Logging.

Using Commons Logging involves just two steps and interaction with two classes:

1. **Get an instance of the `Log` class** using `LogFactory`.
2. **Use the `Log` instance's methods** to write logs.

Here's a simple example:

```java
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Main {
    public static void main(String[] args) {
        Log log = LogFactory.getLog(Main.class);
        log.info("start...");
        log.warn("end.");
    }
}
```

Running the above code will result in a compilation error, as the Commons Logging library (`org.apache.commons.logging`) is not included in the standard Java library. You will need to download the Commons Logging library, find the `commons-logging-1.2.jar` file, and then set up your environment to use this JAR file.

To compile `Main.java` with the JAR, use the following command:

```bash
javac -cp commons-logging-1.2.jar Main.java
```

To run the compiled `Main.class` file, use this command:

```bash
java -cp .;commons-logging-1.2.jar Main
```

On Linux or macOS, use `:` instead of `;` in the classpath:

```bash
java -cp .:commons-logging-1.2.jar Main
```

Commons Logging defines six log levels:

- FATAL
- ERROR
- WARNING
- INFO
- DEBUG
- TRACE

The default level is INFO.

When using Commons Logging in static methods, it's common to define a static log variable:

```java
// Using Log in a static method:
public class Main {
    static final Log log = LogFactory.getLog(Main.class);

    static void foo() {
        log.info("foo");
    }
}
```

In instance methods, you usually define an instance log variable:

```java
// Using Log in an instance method:
public class Person {
    protected final Log log = LogFactory.getLog(getClass());

    void foo() {
        log.info("foo");
    }
}
```

Notice that `getClass()` is used in the instance method to obtain the log. This approach allows subclasses to inherit and use the same log instance.

Additionally, Commons Logging provides useful overloaded methods such as `info(String, Throwable)` for logging exceptions:

```java
try {
    // Some code
} catch (Exception e) {
    log.error("got exception!", e);
}
```

This makes recording exceptions simpler and more straightforward.