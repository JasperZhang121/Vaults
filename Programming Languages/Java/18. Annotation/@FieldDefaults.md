In Java, @FieldDefaults is an annotation provided by Lombok, a popular library that reduces boilerplate code. This annotation is used at the class level to define default modifiers (like private or final) for the fields within that class.

Key Features of @FieldDefaults:
1.	Automatic Modifier Assignment:
	•	You can specify the default access modifier (e.g., private, protected, public) for all fields in the class.
	•	You can also set whether fields should be final.
2.	Simplifies Code:
	•	It eliminates the need to write the same modifier repeatedly for each field.
3.	Customizable Options:
	•	You can set specific modifiers based on the needs of your class, such as private and/or final.

Usage

The annotation is placed at the class level and uses the following attributes:
	•	level: Defines the default access modifier (e.g., AccessLevel.PRIVATE, AccessLevel.PUBLIC).
	•	makeFinal: A boolean that indicates whether all fields in the class should be final.

Example 1: Basic Usage

```java
import lombok.experimental.FieldDefaults;
import lombok.AccessLevel;

@FieldDefaults(level = AccessLevel.PRIVATE, makeFinal = true)
public class User {
    String name;
    int age;
}
```

Result:

This code is equivalent to:

```java
public class User {
    private final String name;
    private final int age;
}
```

Example 2: Without makeFinal

```java
import lombok.experimental.FieldDefaults;
import lombok.AccessLevel;

@FieldDefaults(level = AccessLevel.PRIVATE)
public class Employee {
    String employeeId;
    double salary;
}
```

Result:

This code is equivalent to:

```java
public class Employee {
    private String employeeId;
    private double salary;
}
```

Why Use @FieldDefaults?
	1.	Consistency: Ensures all fields in the class adhere to the same access level or mutability constraints.
	2.	Code Simplification: Removes repetitive declarations, especially in classes with many fields.
	3.	Readability: Makes it easier to quickly understand the default access modifiers for all fields.

Limitations
	1.	The @FieldDefaults annotation applies globally to all fields in the class. You cannot exclude specific fields from its effect unless you explicitly override them in the code.
	2.	It requires Lombok to be configured in your project.

Dependencies

To use @FieldDefaults, you need to have Lombok set up in your project. Add the following Maven dependency:

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.28</version> 
    <scope>provided</scope>
</dependency>
```

Summary

@FieldDefaults is a powerful Lombok annotation that helps enforce uniform field modifiers in Java classes, saving time and reducing errors. It’s ideal for maintaining clean and consistent code, especially when dealing with classes that have many fields.