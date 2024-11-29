The `@JsonProperty` annotation is part of the Jackson library, commonly used in Java for <mark style="background: #FFB8EBA6;">serializing and deserializing JSON data</mark> to Java objects and vice versa. It is typically used with Java classes that will be converted to/from JSON.

### Purpose of `@JsonProperty`

1. **Custom JSON Property Name:** It allows you to map a field in a Java object to a different name in the JSON object. This is particularly useful when the field name in Java does not match the desired JSON property name.
    
2. **Handling Null Values:** You can also use it to define default values or control how `null` values are handled during serialization.
    
3. **Customizing JSON Property Behavior:** It can be used in conjunction with other Jackson annotations to control how serialization and deserialization is performed.

### Example Usage

#### Mapping Java Field to JSON Property:

```java
import com.fasterxml.jackson.annotation.JsonProperty;

public class Person {
    
    @JsonProperty("full_name")  // Maps the Java field 'name' to JSON property 'full_name'
    private String name;
    
    @JsonProperty("age")
    private int age;
    
    // Getters and setters

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

#### Deserializing and Serializing with Jackson:

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class Main {
    public static void main(String[] args) throws Exception {
        // JSON String
        String json = "{\"full_name\":\"John Doe\",\"age\":30}";

        // Create an ObjectMapper instance
        ObjectMapper objectMapper = new ObjectMapper();
        
        // Deserialize JSON to Java Object
        Person person = objectMapper.readValue(json, Person.class);
        System.out.println(person.getName());  // John Doe
        System.out.println(person.getAge());   // 30

        // Serialize Java Object to JSON
        String serializedJson = objectMapper.writeValueAsString(person);
        System.out.println(serializedJson);  // {"full_name":"John Doe","age":30}
    }
}
```

### Key Points

- **JSON Field Name:** The `@JsonProperty("json_field_name")` annotation maps a Java field to a different field name in the resulting JSON.
- **Deserialization & Serialization:** During deserialization (from JSON to Java) and serialization (from Java to JSON), the Jackson library uses this annotation to map the Java fields correctly.
- **Defaults:** If the JSON property name and Java field name match, the annotation is not strictly necessary, but it can still be used for clarity or customizations.
- **Multiple Annotations:** You can use `@JsonProperty` with other Jackson annotations like `@JsonIgnore`, `@JsonInclude`, etc., to control how the property is processed.

This annotation is essential when working with APIs or systems where the JSON structure cannot match the naming conventions used in your Java code (e.g., camelCase in Java and snake_case in JSON).