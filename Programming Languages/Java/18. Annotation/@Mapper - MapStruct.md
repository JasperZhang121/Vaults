
In the context of MapStruct, `@Mapper` is an annotation used to define a mapper interface for converting between different object types (DTOs, entities, etc.) in Java. MapStruct is a code generation library that simplifies the process of mapping one object to another by generating the actual mapping code at compile time. This helps avoid boilerplate code associated with object conversions.

### How `@Mapper` Works

1. **Define the Mapper Interface**: You define a mapping interface and annotate it with `@Mapper`. The interface should declare methods that specify how to map from one object to another.
    
2. **MapStruct's Code Generation**: MapStruct will automatically generate the implementation of this interface during the build process.
    

### Example Usage

Suppose you have a `Person` entity and a `PersonDTO` data transfer object, and you want to map between them.

#### 1. Define the Entity and DTO

```java
public class Person {
    private String name;
    private int age;

    // getters and setters
}

public class PersonDTO {
    private String name;
    private int age;

    // getters and setters
}
```

#### 2. Define the Mapper Interface

```java
import org.mapstruct.Mapper;

@Mapper
public interface PersonMapper {

    // Mapping method from Person to PersonDTO
    PersonDTO personToPersonDTO(Person person);

    // Mapping method from PersonDTO to Person
    Person personDTOToPerson(PersonDTO personDTO);
}
```

#### 3. Use the Mapper in Your Code

Once you define the mapper interface, MapStruct will generate the implementation automatically.

To use the generated implementation, you would inject it like this (if you are using a framework like Spring, you can also inject it as a Spring Bean):

```java
import org.springframework.beans.factory.annotation.Autowired;

public class PersonService {

    @Autowired
    private PersonMapper personMapper;

    public PersonDTO convertPersonToDTO(Person person) {
        return personMapper.personToPersonDTO(person);
    }
}
```

#### 4. Configuration (Optional)

MapStruct can be configured with custom options, such as specifying the component model (`spring`, `cdi`, etc.) and other features like mapping strategy.

```java
@Mapper(componentModel = "spring")
public interface PersonMapper {
    // Methods
}
```

In this case, the mapper will be automatically registered as a Spring Bean.

### Benefits of Using MapStruct

- **Compile-time Code Generation**: MapStruct generates code during the build process, meaning that no runtime reflection is used, leading to better performance.
- **Simplicity**: The syntax is simple and declarative.
- **Customizable**: You can define custom mapping logic using expressions, `@Mapping` annotations, and methods.

### Example with Custom Mapping

If the field names between the entity and DTO differ, you can use the `@Mapping` annotation to specify how to map fields:

```java
@Mapper
public interface PersonMapper {

    @Mapping(source = "name", target = "fullName")
    @Mapping(source = "age", target = "yearsOld")
    PersonDTO personToPersonDTO(Person person);
}
```

This tells MapStruct to map the `name` field from `Person` to `fullName` in `PersonDTO`, and the `age` field to `yearsOld`.

### Conclusion

The `@Mapper` annotation in MapStruct is a simple and powerful way to automate object-to-object mapping with minimal boilerplate code. It helps maintain clean and maintainable code by abstracting out the conversion logic while ensuring that the code is generated at compile-time, resulting in efficient and optimized performance.