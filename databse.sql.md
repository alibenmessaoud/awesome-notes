# SQL Server Basics

#### Section 1. Querying data

-  SELECT – show you how to query data against a single table.

#### Section 2. Sorting data

-  ORDER BY – sort the result set based on values in a specified list of columns

### Section 3. Limiting rows

-  OFFSET FETCH – limit the number of rows returned by a query.
-  SELECT TOP – limit the number of rows or percentage of rows returned in a query’s result set.

#### Section 4. Filtering data

-  [DISTINCT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-select-distinct/) – select distinct values in one or more columns of a table.
-  [WHERE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-where/) – filter rows in the output of a query based on one or more conditions.
-  [AND](https://www.sqlservertutorial.net/sql-server-basics/sql-server-and/) – combine two Boolean expressions and return true if all expressions are true.
-  [OR](https://www.sqlservertutorial.net/sql-server-basics/sql-server-or/)–  combine two Boolean expressions and return true if either of conditions is true.
-  [IN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-in/) – check whether a value matches any value in a list or a subquery.
-  [BETWEEN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-between/) – test if a value is between a range of values.
-  [LIKE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-like/) – check if a character string matches a specified pattern.
-  [Column & table aliases](https://www.sqlservertutorial.net/sql-server-basics/sql-server-alias/) – show you how to use column aliases to change the heading of the query output and table alias to improve the readability of a query.

### Section 5. Joining tables

-  [Joins](https://www.sqlservertutorial.net/sql-server-basics/sql-server-joins/) – give you a brief overview of joins types in SQL Server including inner join, left join, right join and full outer join.
-  [INNER JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-inner-join/) – select rows from a table that have matching rows in another table.
-  [LEFT JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-left-join/) – return all rows from the left table and matching rows from the right table. In case the right table does not have the matching rows, use null values for the column values from the right table.
-  [RIGHT JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-right-join/) – learn a reversed version of the left join.
-  [FULL OUTER JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-full-outer-join/) – return matching rows from both left and right tables, and rows from each side if no matching rows exist.
-  [CROSS JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-cross-join/) – join multiple unrelated tables and create Cartesian products of rows in the joined tables.
-  [Self join](https://www.sqlservertutorial.net/sql-server-basics/sql-server-self-join/) – show you how to use the self-join to query hierarchical data and compare rows within the same table.

### Section 6. Grouping data

-  [GROUP BY](https://www.sqlservertutorial.net/sql-server-basics/sql-server-group-by/)– group the query result based on the values in a specified list of column expressions.
-  [HAVING](https://www.sqlservertutorial.net/sql-server-basics/sql-server-having/) – specify a search condition for a group or an aggregate.
-  [GROUPING SETS](https://www.sqlservertutorial.net/sql-server-basics/sql-server-grouping-sets/) – generates multiple grouping sets.
-  [CUBE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-cube/) – generate grouping sets with all combinations of the dimension columns.
-  [ROLLUP](https://www.sqlservertutorial.net/sql-server-basics/sql-server-rollup/) – generate grouping sets with an assumption of the hierarchy between input columns.

###  Section 7. Subquery

This section deals with the subquery which is a query nested within another statement such as SELECT, INSERT, UPDATE or DELETE statement.

-  [Subquery](https://www.sqlservertutorial.net/sql-server-basics/sql-server-subquery/) – explain the subquery concept and show you how to use various subquery type to select data.
-  [Correlated subquery](https://www.sqlservertutorial.net/sql-server-basics/sql-server-correlated-subquery/) – introduce you to the correlated subquery concept.
-  [EXISTS](https://www.sqlservertutorial.net/sql-server-basics/sql-server-exists/) – test for the existence of rows returned by a subquery.
-  [ANY](https://www.sqlservertutorial.net/sql-server-basics/sql-server-any/) – compare a value with a single-column set of values returned by a subquery and return TRUE the value matches any value in the set.
-  [ALL](https://www.sqlservertutorial.net/sql-server-basics/sql-server-all/) – compare a value with a single-column set of values returned by a subquery and return TRUE the value matches all values in the set.

### Section 8. Set Operators

This section walks you through of using the set operators including union, intersect, and except to combine multiple result sets from the input queries.

-  [UNION](https://www.sqlservertutorial.net/sql-server-basics/sql-server-union/) – combine the result sets of two or more queries into a single result set.
-  [INTERSECT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-intersect/) – return the intersection of the result sets of two or more queries.
-  [EXCEPT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-except/) – find the difference between the two result sets of two input queries.

### Section 9. Common Table Expression (CTE)

-  [CTE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-cte/) – use common table expresssions to make complex queries more readable.
-  [Recursive CTE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-recursive-cte/) – query hierarchical data using recursive CTE.

### Section 10. Pivot

-  [PIVOT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-pivot/) – convert rows to columns

### Section 11. Modifying data

In this section, you will learn how to change the contents of tables in the SQL Server database. The SQL commands for modifying data such as insert, delete, and update are referred to as data manipulation language (DML).

-  [INSERT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-insert/) – insert a row into a table
-  [INSERT multiple rows](https://www.sqlservertutorial.net/sql-server-basics/sql-server-insert-multiple-rows/) – insert multiple rows into a table using a single INSERT statement
-  [INSERT INTO SELECT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-insert-into-select/) – insert data into a table from the result of a query.
-  [UPDATE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-update/) – change the existing values in a table.
-  [UPDATE JOIN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-update-join/) – update values in a table based on values from another table using JOIN clauses.
-  [DELETE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-delete/) – delete one or more rows of a table.
-  [MERGE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-merge/) – walk you through the steps of performing a mixture of insertion, update, and deletion using a single statement.

### Section 12. Data definition

This section shows you how to manage the most important database objects including databases and tables.

-  [CREATE DATABASE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-create-database/) – show you how to create a new database in a SQL Server instance using the CREATE DATABASE statement and SQL Server Management Studio.
-  [DROP DATABASE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-drop-database/) – learn how to delete existing databases.
-  [CREATE SCHEMA](https://www.sqlservertutorial.net/sql-server-basics/sql-server-create-schema/) – describe how to create a new schema in a database.
-  [ALTER SCHEMA](https://www.sqlservertutorial.net/sql-server-basics/sql-server-alter-schema/) – show how to transfer a securable from a schema to another within the same database.
-  [DROP SCHEMA](https://www.sqlservertutorial.net/sql-server-basics/sql-server-drop-schema/) – learn how to delete a schema from a database.
-  [CREATE TABLE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-create-table/) – walk you through the steps of creating a new table in a specific schema of a database.
-  [Identity column](https://www.sqlservertutorial.net/sql-server-basics/sql-server-identity/) – learn how to use the IDENTITY property to create the identity column for a table.
-  [Sequence](https://www.sqlservertutorial.net/sql-server-basics/sql-server-sequence/) – describe how to generate a sequence of numeric values based on a specification.
-  [ALTER TABLE ADD column](https://www.sqlservertutorial.net/sql-server-basics/sql-server-alter-table-add-column/) – show you how to add one or more columns to an existing table
-  [ALTER TABLE ALTER COLUMN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-alter-table-alter-column/) – show you how to change the definition of existing columns in a table.
-  [ALTER TABLE DROP COLUMN](https://www.sqlservertutorial.net/sql-server-basics/sql-server-alter-table-drop-column/) – learn how to drop one or more columns from a table.
-  [Computed columns](https://www.sqlservertutorial.net/sql-server-basics/sql-server-computed-columns/) – how to use the computed columns to resue the calculation logic in multiple queries.
-  [DROP TABLE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-drop-table/) – show you how to delete tables from the database.
-  [TRUNCATE TABLE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-truncate-table/) – delete all data from a table faster and more efficiently.
-  [SELECT INTO](https://www.sqlservertutorial.net/sql-server-basics/sql-server-select-into/) – learn how to create a table and insert data from a query into it.
-  [Rename a table](https://www.sqlservertutorial.net/sql-server-basics/sql-server-rename-table/) –  walk you through the process of renaming a table to a new one.
-  [Temporary tables](https://www.sqlservertutorial.net/sql-server-basics/sql-server-temporary-tables/) – introduce you to the temporary tables for storing temporarily immediate data in stored procedures or database session.
-  [Synonym](https://www.sqlservertutorial.net/sql-server-basics/sql-server-synonym/) – explain you the synonym and show you how to create synonyms for database objects.

### Section 13. SQL Server Data Types

-  [SQL Server data types](https://www.sqlservertutorial.net/sql-server-basics/sql-server-data-types/) – give you an overview of the built-in SQL Server data types.
-  [BIT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-bit/) – store bit data i.e., 0, 1, or NULL in the database with the BIT data type.
-  [INT](https://www.sqlservertutorial.net/sql-server-basics/sql-server-int/) – learn about various integer types in SQL server including BIGINT, INT, SMALLINT, and TINYINT.
-  [DECIMAL](https://www.sqlservertutorial.net/sql-server-basics/sql-server-decimal/) – show you how to store exact numeric values in the database by using DECIMAL or NUMERIC data type.
-  [CHAR](https://www.sqlservertutorial.net/sql-server-basics/sql-server-char/) – learn how to store fixed-length, non-Unicode character string in the database.
-  [NCHAR](https://www.sqlservertutorial.net/sql-server-basics/sql-server-nchar/) – show you how to store fixed-length, Unicode character strings and explain the differences between `CHAR` and `NCHAR` data types
-  [VARCHAR](https://www.sqlservertutorial.net/sql-server-basics/sql-server-varchar/) – store variable-length, non-Unicode string data in the database.
-  [NVARCHAR](https://www.sqlservertutorial.net/sql-server-basics/sql-server-nvarchar/) – learn how to store variable-length, Unicode string data in a table and understand the main differences between VARCHAR and NVARCHAR.
-  [DATETIME2](https://www.sqlservertutorial.net/sql-server-basics/sql-server-datetime2/) – illustrate how to store both date and time data in a database.
-  [DATE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-date/) – discuss the date data type and how to store the dates in the table.
-  [TIME](https://www.sqlservertutorial.net/sql-server-basics/sql-server-time/) – show you how to store time data in the database by using the TIME data type.
-  [DATETIMEOFFSET](https://www.sqlservertutorial.net/sql-server-basics/sql-server-datetimeoffset/) – show you how to manipulate datetime with the time zone.
-  [GUID](https://www.sqlservertutorial.net/sql-server-basics/sql-server-guid/) – learn about the GUID and how to use the NEWID() function to generate GUID values.

### Section 14. Constraints

-  [Primary key](https://www.sqlservertutorial.net/sql-server-basics/sql-server-primary-key/) – explain you to the primary key concept and show you how to use the primary key constraint to manage a primary key of a table.
-  [Foreign key](https://www.sqlservertutorial.net/sql-server-basics/sql-server-foreign-key/) – introduce you to the foreign key concept and show you use the `FOREIGN KEY` constraint to enforce the link of data in two tables.
-  [NOT NULL constraint](https://www.sqlservertutorial.net/sql-server-basics/sql-server-not-null-constraint/) – show you how to ensure a column not to accept NULL.
-  [UNIQUE constraint](https://www.sqlservertutorial.net/sql-server-basics/sql-server-unique-constraint/) – ensure that data contained in a column, or a group of columns, is unique among rows in a table.
-  [CHECK constraint](https://www.sqlservertutorial.net/sql-server-basics/sql-server-check-constraint/) – walk you through the process of adding logic for checking data before storing them in tables.

### Section 15. Expressions

-  [CASE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-case/) – add if-else logic to SQL queries by using simple and searched CASE expressions.
-  [COALESCE](https://www.sqlservertutorial.net/sql-server-basics/sql-server-coalesce/) – handle NULL values effectively using the COALESCE expression.
-  [NULLIF](https://www.sqlservertutorial.net/sql-server-basics/sql-server-nullif/) – return NULL if the two arguments are equal; otherwise, return the first argument.

### Section 16. Useful Tips

-  [Find duplicates](https://www.sqlservertutorial.net/sql-server-basics/sql-server-find-duplicates/) – show you how to find duplicate values in one or more columns of a table.
-  [Delete duplicates](https://www.sqlservertutorial.net/sql-server-basics/delete-duplicates-sql-server/) – describe how to remove duplicate rows from a table.