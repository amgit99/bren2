#### Constraints :
`NOT NULL` ensures a column will never have a `null` value.
`DEFAULT` this value is assigned when no other value is assigned.
`UNIQUE` this ensures every value of this column will be unique.
`PRIMARY KEY` combination of `NOT NULL` and `UNIQUE`, used for identification
#### Create table : 
```SQL
CREATE TABLE table_name(
	column1 datatype constraint,
	column2 datatype constraint,
	columnN datatype constraint,
		...
	PRIMARY KEY(column_i) // another way to write PK constraint
);
```
#### Insert : 
```SQL
INSERT INTO table_name VALUES(
	col1_value, col2_value, ... ,colN_value
);
```
#### Select : 
```SQL
SELECT columnI, columnJ, ... , columnL
FROM table_name;
```
`*` is used to select all columns.
`DISTINCT` keyword follows `SELECT` when distinct values for <span style="color:#e1db3d">tuples</span> is the result is needed.
#### Operators : 
`AND`, `OR`, `NOT` are used in the `WHERE` clause to operate on conditions. `NOT` is unary.
`LIKE` is used for string matching, the `%` character is used for matching any number of characters, while the `_` is used for matching just one character.
`BETWEEN` to get values where they lie in some range.
```SQL
SELECT * FROM employee
WHERE e_name LIKE 'J%'        // Starts with J

SELECT * FROM employee
WHERE e_age BETWEEN 25 AND 35 // inclusive for ranges
```
#### Aggregation Functions : 
These functions take a set or multiset of values and <span style="color:#e1db3d">return a single value</span> for that group.
`AVG`, `MIN`, `MAX`, `SUM`, `COUNT` are the 5 basic standard built in aggregate functions.
Duplicates are retained as it is important for correctness, as in the case of `AVG`.
These can be applied on the groups that are created by the `GROUP BY` clause, returning one value per group. This puts a constraint on the attributes that can be present in the `SELECT` clause. The attributes that are present in `SELECT` clause must be either, <span style="color:#e1db3d">an argument to the aggregate function or must be present in the grouping condition</span>.
#### String Functions : 
`LTRIM(str)` this removes all the leading spaces of the string.
`LOWER(str)` makes all uppercase to lowercase.
`UPPER(str)` makes all lowercase to uppercase.
`REVERSE(str)` reverses the string.
`SUBSTRING(str, index, len)` gives the substring specified by 0 indexed offset and length.
#### Top :
```SQL
SELECT TOP N * FROM employee
ORDER BY e_age IN DESC
```

#### Joins :
###### Inner Join : 
```SQL
SELECT table1.colI, table2.colJ
FROM table1 INNER JOIN table2
ON table1.colK = table2.colL
WHERE ...
```
###### LEFT/RIGHT Join : 
``` sql
SELECT table1.colI, table2.colJ
FROM table1 LEFT JOIN table2
ON table1.colK = table2.colL
WHERE ...
```
The update command is used with join for complex updates
#### Views :
```SQL
CREATE VIEW view_name AS
{SQL QUERY}
```