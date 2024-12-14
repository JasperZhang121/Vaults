
`@GetMapping` is used to handle HTTP `GET` requests in a Spring application. It is a shorthand for `@RequestMapping` with `method = RequestMethod.GET`. This annotation maps a specific URL or path to a method in a controller.

#### Example:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/hello")
    public String sayHello(@RequestParam(defaultValue = "World") String name) {
        return "Hello, " + name + "!";
    }
}
```

**Explanation:**

- **URL Mapping**: Handles GET requests to `/hello`.
- **Query Parameters**: Reads the `name` parameter from the request URL (e.g., `/hello?name=John`).

**Sample URL**:

```
http://localhost:8080/hello?name=John
```

**Response**:

```
Hello, John!
```
