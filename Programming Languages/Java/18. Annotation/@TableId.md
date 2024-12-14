`@TableId` is an annotation used in MyBatis Plus to mark a field as the primary key of a table. It is used for automatic identification of primary keys in CRUD operations. By default, MyBatis Plus uses this annotation to handle the primary key strategy, and it can be configured to use either auto-increment or UUID as the primary key type.

### Example Usage:

```java
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;

@TableName("user") // Table name
public class User {

    @TableId(value = "id", type = IdType.AUTO) // Primary key with auto-increment strategy
    private Long id;

    private String name;
    private Integer age;

    // Getters and setters
}
```

### Key Parameters:

1. **value**: Specifies the column name in the database for the primary key.
2. **type**: Defines the strategy for generating the primary key:
    - **IdType.AUTO**: Auto-incremented (common for SQL databases).
    - **IdType.UUID**: UUID-based primary key.
    - **IdType.INPUT**: Input-based, where the user provides the ID value.
    - **IdType.ID_WORKER**: Uses a snowflake algorithm for generating unique IDs.

#### Default Behavior:

- If not specified, MyBatis Plus will try to automatically determine the primary key based on the naming convention (e.g., if the primary key field is named `id`, it will map to the `id` column in the table).
- The `type` can be explicitly set if a specific ID generation strategy is needed.
