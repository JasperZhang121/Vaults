### Basic Usage of @RequestMapping

@RequestMapping is used to map HTTP requests to controller methods. Key characteristics include:

- Represents a shared mapping across HTTP methods
- Can be used at both class and method levels
- By default, accepts all HTTP request methods (GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE, CONNECT)

#### Basic Example
```java
@RestController
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/login")  // Typically use @GetMapping for retrieving data
    public String login() {
        return "user login";
    }
    
    @RequestMapping("/register")  // Typically use @PostMapping for submitting data
    public String register() {
        return "user register";
    }
}
```

### Handling HTTP Methods

#### HTTP Method Mapping Annotations

| Request | Combined Annotation | Shared Annotation |
|---------|---------------------|-------------------|
| GET | @GetMapping | @RequestMapping(method = RequestMethod.GET) |
| POST | @PostMapping | @RequestMapping(method = RequestMethod.POST) |
| PUT | @PutMapping | @RequestMapping(method = RequestMethod.PUT) |
| DELETE | @DeleteMapping | @RequestMapping(method = RequestMethod.DELETE) |
| PATCH | @PatchMapping | @RequestMapping(method = RequestMethod.PATCH) |

**Best Practice**: Use combined annotations for clearer code and reduced configuration.

### Multiple URI Mapping

You can map multiple request paths to a single method:

```java
@RequestMapping(value = {
    "",
    "/page",
    "page*",
    "view/*,**/msg"
})
```

#### Path Matching Patterns
- `"/resources/ima?e.p ang "` - Matches single character
- `"/resources/*.png"` - Matches zero or more characters
- `"/resources/**"` - Matches multiple path segments
- `"/projects/{project}/versions"` - Captures path variable
- `"/projects/{project:[a-z]+}/versions"` - Regex path variable matching

### Request Parameters with @RequestParam

#### Properties of @RequestParam

1. **`value`**: Specify request parameter mapping
```java
@RequestMapping("/id")
String getIdByValue(@RequestParam("id") String personId)
```

2. **`required`**: Define parameter requirement
```java
@RequestMapping("/name")
String getName(@RequestParam(value = "person", required = true) String personName)
```

3. **`defaultValue`**: Provide default parameter value
```java
@RequestMapping("/name")
String getName(@RequestParam(value = "person", defaultValue = "John") String personName)
```

### Producing and Consuming Objects

#### Using `produces` and `consumes`

```java
@RequestMapping(value = "/prod", produces = {"application/JSON"})
@ResponseBody
String getProduces() {
    return "Produces JSON";
}

@RequestMapping(value = "/cons", consumes = {"application/JSON", "application/XML"})
String getConsumes() {
    return "Consumes JSON/XML";
}
```

### Handling Request Headers

```java
@RequestMapping(value = "/head", headers = {
    "content-type=text/plain",
    "content-type=text/html"
})
```

### Request Parameter Filtering

```java
@RequestMapping(value = "/fetch", params = {"personId=10"})
String getParams(@RequestParam("personId") String id)
```

### Dynamic URI Handling

```java
@RequestMapping(value = "/fetch/{id}", method = RequestMethod.GET)
String getDynamicUriValue(@PathVariable String id)
```

### Default Processing Method

```java
@RequestMapping()
String defaultMethod() {
    return "Default method";
}
```

**Note**: `@PathVariable` differs from `@RequestParam` - it extracts values from the URI path, while `@RequestParam` gets values from the URI template.