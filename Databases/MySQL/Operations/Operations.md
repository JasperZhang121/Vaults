---

---
### 0. **Create Table**

1. `CREATE TABLE` – Used to start the creation of a new table.
   
2. `IF NOT EXISTS` – Checks if a table already exists and only creates it if it doesn’t.

3. Column Definition:
   - `COLUMN_NAME DATA_TYPE` – Specifies the column name and its data type (e.g., `id INT`).
   - `NOT NULL` – Ensures that the column cannot have NULL values.
   - `DEFAULT` – Specifies a default value for the column.
   - `AUTO_INCREMENT` – Automatically increments the value of the column for each new row (usually for primary keys).
   - `UNIQUE` – Ensures all values in the column are unique.
   - `PRIMARY KEY` – Defines the primary key for the table.
   - `FOREIGN KEY` – Defines a foreign key constraint to link to another table.
   - `CHECK` – Sets a condition that column values must satisfy.

4. Indexes:
   - `PRIMARY KEY (COLUMN_NAME)` – Defines a primary key on the specified column.
   - `UNIQUE (COLUMN_NAME)` – Defines a unique index on the column.
   - `INDEX (COLUMN_NAME)` – Creates an index on the column for faster searches.

5. Constraints:
   - `FOREIGN KEY (COLUMN_NAME) REFERENCES OTHER_TABLE(OTHER_COLUMN)` – Sets a foreign key constraint linking to another table.
   - `ON DELETE CASCADE` / `ON UPDATE CASCADE` – Specifies what happens when a referenced row is deleted or updated.
   
6. Table Options:
   - `ENGINE=ENGINE_TYPE` – Specifies the storage engine (e.g., `ENGINE=InnoDB`).
   - `CHARSET=CHARACTER_SET` – Defines the default character set for the table.
   - `COLLATE=COLLATION` – Defines the collation for sorting and comparison.
   - `COMMENT 'table comment'` – Adds a comment describing the table.

---
### 1. **Basic CRUD Operations (Create, Read, Update, Delete)**

- **CREATE**: Create a new table to record data.
- **INSERT**: Add new records to a table.
- **SELECT**: Retrieve data from one or more tables.
- **UPDATE**: Modify existing records in a table.
- **DELETE**: Remove records from a table.

### 2. **Joins and Data Relationships**

- **INNER JOIN**: Select records that have matching values in both tables.
- **LEFT JOIN**: Select all records from the left table and the matched records from the right table.
- **RIGHT JOIN**: Select all records from the right table and the matched records from the left table.
- **FULL JOIN**: Select all records when there is a match in either left or right table.
- **CROSS JOIN**: Return the Cartesian product of the sets of rows from the joined tables.
- **SELF JOIN**: Join a table to itself.

### 3. **Filtering and Sorting Data**

- **WHERE**: Filter records based on conditions.
- **HAVING**: Filter records after grouping with `GROUP BY`.
- **ORDER BY**: Sort results in ascending or descending order.
- **LIMIT**: Restrict the number of records returned by a query.
- **OFFSET**: Skip a number of rows before returning the results.
- **DISTINCT**: Select unique records from a table.
  
### 4. **Aggregate Functions**

- **COUNT()**: Count the number of rows.
- **SUM()**: Calculate the total sum of a numeric column.
- **AVG()**: Calculate the average value of a numeric column.
- **MIN()**: Get the smallest value from a column.
- **MAX()**: Get the largest value from a column.
- **GROUP BY**: Group rows that have the same values in specified columns.

### 5. **Data Manipulation**

- **TRUNCATE**: Remove all records from a table, but keep the structure.
- **ALTER**: Modify an existing table (add, drop columns, modify column data types).
- **REPLACE**: Insert a record or update an existing one if it already exists.
- **MERGE**: Combine records from two tables based on certain conditions.
- **UPSERT (INSERT ON DUPLICATE KEY UPDATE)**: Insert a record, or update if it exists.

### 6. **Transaction Management**

- **START TRANSACTION**: Begin a transaction.
- **COMMIT**: Save all changes made during the current transaction.
- **ROLLBACK**: Undo changes made during the current transaction.
- **SAVEPOINT**: Set a point within a transaction that you can roll back to.
- **LOCK/UNLOCK TABLES**: Manually lock or unlock tables to control concurrency.

### 7. **Views and Virtual Tables**

- **CREATE VIEW**: Create a virtual table based on a query.
- **ALTER VIEW**: Modify an existing view.
- **DROP VIEW**: Delete a view.

### 8. **Index Management**

- **CREATE INDEX**: Create an index to improve query performance.
- **DROP INDEX**: Delete an index.
- **SHOW INDEX**: View information about indexes.

### 9. **Database and Table Management**

- **CREATE DATABASE**: Create a new database.
- **DROP DATABASE**: Delete a database.
- **CREATE TABLE**: Create a new table.
- **DROP TABLE**: Delete a table.
- **ALTER TABLE**: Modify an existing table.
- **SHOW TABLES**: List all tables in the database.

### 10. **Security and Permissions**

- **CREATE USER**: Create a new MySQL user.
- **DROP USER**: Remove a user.
- **GRANT**: Grant privileges to a user.
- **REVOKE**: Remove privileges from a user.
- **SET PASSWORD**: Change the password of a MySQL user.
- **SHOW GRANTS**: Display the privileges granted to a user.

### 11. **Backup and Restoration**

- **mysqldump**: Command-line utility to back up databases.
- **LOAD DATA INFILE**: Import data from an external file into a table.
- **SELECT INTO OUTFILE**: Export query results to an external file.
- **RESTORE**: Restore data from a backup.

### 12. **Data Import/Export**

- **LOAD DATA INFILE**: Import large amounts of data efficiently.
- **SELECT INTO OUTFILE**: Export the results of a query into a file.

### 13. **Stored Procedures and Functions**

- **CREATE PROCEDURE**: Create a stored procedure.
- **CALL**: Execute a stored procedure.
- **CREATE FUNCTION**: Create a stored function.
- **DROP PROCEDURE**: Delete a stored procedure.
- **DROP FUNCTION**: Delete a stored function.

### 14. **Triggers**

- **CREATE TRIGGER**: Automatically execute a query when an event occurs (e.g., BEFORE/AFTER INSERT, UPDATE, DELETE).
- **DROP TRIGGER**: Delete a trigger.

### 15. **Replication and Sharding**

- **SET GLOBAL server_id**: Set a unique server ID for replication.
- **SHOW MASTER STATUS**: Show the status of the binary log on the master server.
- **CHANGE MASTER TO**: Set up replication with a master server.
- **START SLAVE**: Start replication on a slave server.
- **STOP SLAVE**: Stop replication on a slave server.
  
### 16. **Optimization and Performance Tuning**

- **ANALYZE TABLE**: Analyze and optimize table performance.
- **OPTIMIZE TABLE**: Reorganize and reclaim space.
- **EXPLAIN**: Show how MySQL executes a query (for optimization purposes).
- **SHOW STATUS**: Display server status information.
- **SHOW PROCESSLIST**: View active MySQL processes.
  
### 17. **Concurrency and Locking**

- **LOCK TABLES**: Manually lock tables for a session.
- **UNLOCK TABLES**: Unlock tables.
- **SHOW OPEN TABLES**: Show which tables are currently in use.

### 18. **Data Integrity and Constraints**

- **FOREIGN KEY**: Establish a relationship between two tables.
- **CHECK**: Apply a condition on column data.
- **DEFAULT**: Set a default value for a column if none is provided.
- **AUTO_INCREMENT**: Automatically increment a numeric column for each new row.
  
### 19. **Schema Information**

- **SHOW TABLE STATUS**: Display the status of tables.
- **SHOW COLUMNS**: Display column details for a specific table.
- **DESCRIBE**: Get information about the structure of a table.

### 20. **Miscellaneous**

- **SET**: Modify system variables.
- **SHOW VARIABLES**: Display MySQL system variables.
- **USE DATABASE**: Switch between databases.