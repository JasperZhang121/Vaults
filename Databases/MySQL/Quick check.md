
| Keyword | Explanation |
| ---- | ---- |
| `Select` | Retrieves data from one or more tables. |
| `Distinct` | Removes duplicate rows from the result set. |
| `Where` | Filters rows based on specified conditions. |
| `Order By` | Sorts the result set in ascending or descending order. |
| `And` | Combines two or more conditions in a WHERE clause. |
| `Or` | Allows either of two conditions to be met in a WHERE clause. |
| `Not` | Negates a condition in a WHERE clause. |
| `Insert Into` | Adds new rows to a table. |
| `Null Values` | Handles missing or unknown data in a table. |
| `Update` | Modifies existing data in a table. |
| `Delete` | Removes rows from a table. |
| `Select Top` | Limits the number of rows returned in a result set. |
| `Min and Max` | Finds the minimum or maximum value in a column. |
| `Count` | Counts the number of rows that match a specified condition. |
| `Sum` | Calculates the total sum of a numeric column. |
| `Avg` | Calculates the average value of a numeric column. |
| `Like` | Searches for a specified pattern in a column. |
| `Wildcards` | Used with LIKE to search for patterns. |
| `In` | Allows specifying multiple values in a WHERE clause. |
| `Between` | Selects values within a given range. |
| `Aliases` | Renames a table or column for the duration of a query. |
| `Joins` | Combines rows from two or more tables. |
| `Inner Join` | Returns rows when there is a match in both tables. |
| `Left Join` | Returns all rows from the left table, and matched rows from the right table. |
| `Right Join` | Returns all rows from the right table, and matched rows from the left table. |
| `Full Join` | Returns rows when there is a match in one of the tables. |
| `Self Join` | Joins a table to itself. |
| `Union` | Combines the result set of two or more SELECT statements. |
| `Group By` | Groups rows that have the same values in specified columns. |
| `Having` | Used with GROUP BY to filter groups based on a condition. |
| `Any, All` | Compares a value to each value in a list (ANY) or to every value in a list (ALL). |
| `Select Into` | Copies data from one table into a new table. |
| `Insert Into Select` | Inserts data from one table into another. |
| `Case` | Provides if-then-else type of logic to SQL. |
| `Null Functions` | Functions that handle NULL values (like ISNULL, COALESCE). |
| `Stored Procedures` | A set of SQL queries thatcan be stored and executed in the database. |
| `Comments` | Allows adding comments in SQL code for readability. |
| `Operators` | Used to perform operations on data (like arithmetic, logical, etc.). |

## String Functions

| Function | Description |
|----------|-------------|
| `CONCAT(string1, string2, ...)` | Concatenates two or more strings into one string. |
| `LENGTH(string)` | Returns the length of a string. |
| `LOWER(string)` | Converts all characters in a string to lowercase. |
| `UPPER(string)` | Converts all characters in a string to uppercase. |
| `TRIM(string)` | Removes whitespace from both ends of a string. |
| `LTRIM(string)` | Removes leading whitespace from a string. |
| `RTRIM(string)` | Removes trailing whitespace from a string. |
| `SUBSTRING(string, start, length)` | Extracts a substring from a string (starting at `start`, `length` characters long). |
| `REPLACE(string, from_string, to_string)` | Replaces all occurrences of a specified string with another string. |
| `CHAR_LENGTH(string)` | Returns the number of characters in a string. |
| `CHARINDEX(substring, string)` | Returns the position of a substring within a string. |
| `POSITION(substring IN string)` | Similar to `CHARINDEX`, it returns the position of a substring within a string. |
| `LEFT(string, number_of_chars)` | Returns the left part of a string with the specified number of characters. |
| `RIGHT(string, number_of_chars)` | Returns the right part of a string with the specified number of characters. |
| `REVERSE(string)` | Reverses a string. |

## Date Functions

| Function | Description |
|----------|-------------|
| `CURRENT_DATE` | Returns the current date. |
| `CURRENT_TIMESTAMP` | Returns the current date and time. |
| `DATEADD(datepart, number, date)` | Adds a specified number of units to a date. |
| `DATEDIFF(datepart, startdate, enddate)` | Calculates the difference between two dates. |
| `DAY(date)` | Returns the day of the month for a specified date. |
| `MONTH(date)` | Returns the month from a specified date. |
| `YEAR(date)` | Returns the year from a specified date. |
| `DATE_FORMAT(date, format)` | Formats a date as specified. |
| `GETDATE()` | Returns the current date and time (SQL Server). |
| `SYSDATE` | Returns the current date and time (Oracle). |
| `TO_DATE(string, format)` | Converts a string to a date, using the specified format. |
| `EXTRACT(part FROM date)` | Extracts and returns the value of a specified part of a date (e.g., year, month, day). |
| `LAST_DAY(date)` | Returns the last day of the month for a specified date. |
| `NOW()` | Returns the current date and time (MySQL). |
