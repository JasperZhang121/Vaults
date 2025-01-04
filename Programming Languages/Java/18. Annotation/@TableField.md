`@TableField` is an annotation provided by **MyBatis Plus**, an enhancement library for MyBatis. It is primarily used to map Java class fields to corresponding columns in a database table. This annotation provides detailed configuration for how a specific field in a class should be mapped to the database.

### 1. Purpose of `@TableField`

- Specifies the mapping of a class field to a database column.
- Customizes field behavior, such as inclusion in INSERT or UPDATE operations.
- Helps map database fields that differ in name or format from the Java fields.

### 2. Common Attributes

|Attribute|Description|Example Value|
|---|---|---|
|`value`|Specifies the database column name that the Java field maps to.|`"user_name"`|
|`exist`|Indicates whether the field exists in the database table. `false` excludes it from SQL mapping.|`true` or `false`|
|`select`|Specifies if the field should be included in SELECT statements.|`true` or `false`|
|`insertStrategy`|Defines the strategy for including the field in INSERT operations.|`FieldStrategy.NOT_NULL`|
|`updateStrategy`|Defines the strategy for including the field in UPDATE operations.|`FieldStrategy.NOT_EMPTY`|
|`fill`|Configures automatic field filling, such as `INSERT`, `UPDATE`, or both.|`FieldFill.INSERT_UPDATE`|

### 3. Example Usage

#### Basic Mapping

If a field name in the Java class is different from the column name in the database:

```java
@TableField(value = "user_name")
private String userName;
```

- The `userName` field in the Java class maps to the `user_name` column in the database.

#### Exclude Field from Mapping

If a field should not be included in the database operations (e.g., a helper field):

```java
@TableField(exist = false)
private String temporaryData;
```

- The `temporaryData` field is excluded from SQL queries and operations.

#### Auto-Filling Fields

For fields that are automatically filled during INSERT or UPDATE operations (e.g., timestamps):

```java
@TableField(fill = FieldFill.INSERT)
private LocalDateTime createdAt;

@TableField(fill = FieldFill.INSERT_UPDATE)
private LocalDateTime updatedAt;
```

- `createdAt` is filled only during INSERT.
- `updatedAt` is filled during both INSERT and UPDATE.

#### Conditional Inclusion in SQL

Control when a field is included in SQL using strategies:

```java
@TableField(insertStrategy = FieldStrategy.NOT_NULL)
private String email;
```

- The `email` field is included in INSERT operations only if it is not `null`.

### 4. Notes

- `@TableField` is only effective when using MyBatis Plus with its enhanced features.
- Default behavior without `@TableField` assumes the field name matches the column name exactly.

### 5. Practical Example

A complete entity class using `@TableField`:

```java
@TableName("user_table") // Maps the class to the table "user_table"
public class User {
    
    @TableId(value = "id", type = IdType.AUTO) // Primary key
    private Long id;
    
    @TableField(value = "user_name")
    private String userName; // Maps to "user_name"
    
    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createdAt; // Auto-filled during INSERT
    
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updatedAt; // Auto-filled during INSERT and UPDATE
    
    @TableField(exist = false)
    private String tempField; // Not mapped to any database column
}
```
