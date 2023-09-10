 # SQL Notes
-------

### INTERACTING WITH A TABLES

- 'SELECT' - selecting data
```SQL
SELECT column(s) FROM table;
```


- 'SELECT *' - selecting all columns
```SQL
SELECT * FROM table;
```

- 'SELECT DISTINCT' - selecting unique data
```SQL
SELECT DISTINCT(column(s)) FROM table;
```

- 'COUNT' - number of rows
```SQL
SELECT COUNT(column(s) or *) FROM table; 
```

- 'SELECT WHERE' - selecting data with conditions
```SQL
SELECT column(s) FROM table
WHERE (condition);
-- (>, >=, <, <=, =, AND, OR) --
```

- 'ORDER BY' - sorting / ordering data
```SQL
SELECT column(s) FROM table
ORDER BY (ASC, DESC);
```

- 'LIMIT' - limiting the amount of rows given back
```SQL
SELECT column(s) FROM table
-- any number of lines -- 
LIMIT 5;
```

- 'BETWEEN' - finding data between certain values
```SQL
SELECT column(s) FROM table
WHERE column(s) BETWEEN value1 AND value2;
```

- 'IN' - multiple value options
```SQL
SELECT column(s) FROM table
WHERE column IN (value1, value2, value3);
```

- 'LIKE' - pattern matching case sensitive (% = any number of characters, _ = single character)
```sql
SELECT column FROM table
WHERE column LIKE '(%, _, other characters)'
```


- 'ILIKE' - pattern matching not case sensitive (% = any number of characters, _ = single character)
```sql
SELECT column FROM table
WHERE column ILIKE '(%, _, other characters)'
```
- Aggregation Functions
```sql
SELECT MAX(column) FROM table;
SELECT MIN(column) FROM table;
SELECT AVG(column) FROM table;
SELECT SUM(column) FROM table;
SELECT COUNT(column) FROM table;
SELECT ROUND((AVG(column) FROM table), (# of decimal places)
```
- 'GROUP BY' - grouping data with categories
```SQL
SELECT category_col, AGG(data_col) FROM table
**add WHERE statement here**
GROUP BY category_col;
**add ORDER BY here**
**add LIMIT here**
```

- 'HAVING' - allows filtering with AGG functions
```sql
SELECT company, SUM(SALES) FROM finance_table
GROUP BY company
HAVING SUM(sales) > 100;
```

- 'AS' - renaming displayed columns
```sql
SELECT column AS new_name FROM table;
```

- 'INNER JOIN' - outputs the intersection of the rows from the given columns from different tables
```sql
SELECT * FROM TableA 
INNER JOIN TableB
ON TableA.col_match = TableB.col_match;
```

- 'FULL OUTER JOIN' - outputs ALL of the rows from the the given columns from different tables
```sql
SELECT * FROM TableA
FULL OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;

**using WHERE**

SELECT * FROM TableA
FULL OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;
WHERE TableA.id IS null
OR TableB.id IS null;
```

- 'LEFT OUTER JOIN' - everything exclusive to TableA and anything from TableB which matches to something in TableA
```sql
SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;

**using WHERE**

SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;
WHERE TableB.id IS null;
```

- 'RIGHT OUTER JOIN' - everything exclusive to TableB and anything from TableA which matches to something in TableB
```sql
SELECT * FROM TableA
RIGHT OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;

**using WHERE**

SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.col_match = TableB.col_match;
WHERE TableA.id IS null;
```

- 'UNION' - taking all rows from two tables and stacking them
```sql
SELECT * FROM TableA
UNION
SELECT * FROM TableB;
```

- 'SUB QUERY' - a query call within a query
```sql
SELECT column FROM table
WHERE (...) (SELECT column FROM table...WHERE condition);
```

- 'EXISTS' - checking if the sub query actually returned something
```sql
SELECT column FROM table
WHERE EXISTS (...) (SELECT column FROM table...WHERE condition);
```

- 'SELF JOIN' - Joining a table with a duplicate of itself
```sql
SELECT tableA.col, tableB.col
FROM table AS TableA
INNER JOIN table AS TableB
ON tableA.col = tableB.col;
```

-----------
### MAKING TABLES

- TYPE KEY WORDS:
```sql
SERIAL - assigns a unique number to each row
VARCHAR(length) - basically a string, and given length
TIMESTAMP - date and time
DATE - date
SMALLINT
INTEGER
BIGINT
```
  
- CONSTRAINT KEY WORDS:
```sql
PRIMARY KEY - given to the primary column name (e.g., user_id)
UNIQUE - all unique inputs (e.g., username)
NULL - it must be blank
NOT NULL - it cannot be blank
CHECK (condition) - basically a WHERE or IF 
```

- 'CREATE TABLE' - creating a general table
```sql
CREATE TABLE table_name (
  column_name TYPE column_constraint, 
  column_name TYPE column_constraint,
  table_constraint table_constraint
  ) INHERITS existing_table_name;
```

- 'REFERENCES' - creating a table which references to another column from a different table (foreign key)
```sql
CREATE TABLE table_name (
  referenced_column TYPE REFERENCES referenced_table(referenced_column)
  );
```

- 'INSERT' - add in rows into a column
```sql
INSERT INTO table (column1, column2,...)
VALUES
(value1, value2...),
(value1, value2...),...,;
  
  **insert from another table**
  
INSERT INTO table(column1, column2,...)
SELECT column1, column2
FROM another_table
WHERE condition;
```

- 'UPDATE' - allows to change current values in columns
```sql
UPDATE table
SET column1 = value1, 
    column2 = value2,...,
WHERE condition;

**update using another tables values**

UPDATE TableA
SET original_column = TableB.new_col 
FROM tableB
WHERE tableA.id = tableB.id;
```

- 'DELETE' - deleting rows
```sql
DELETE FROM table
WHERE row_id = 1;

**delete rows based on their presence in other tables**

DELETE FROM tableA
USING tableB
WHERE tableA.id = tableB.id;

**delete all rows from table**

DELETE FROM table;
```

- 'ALTER' - allows for changes in an existing table
```sql
ALTER TABLE table_name
ADD COLUMN new_col TYPE;

ALTER TABLE table_name
DROP COLUMN IF EXISTS col_name CASCADE(if you wanna remove all dependencies);

ALTER TABLE table_name
ALTER COLUMN col_name
SET DEFAULT value/DROP DEFAULT value/SET NOT NULL/DROP NOT NULL/ADD CONSTRAINT constraint_name;

ALTER TABLE table_name
RENAME TO new_table_name;

ALTER TABLE table_name
RENAME COLUMN column_name TO new_column_name;
```
### CONDITIONAL EXPRESSIONS AND KEY WORDS

- 'CASE' - if/else clause
```sql
SELECT column,
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE some_other_result
END
FROM table;
```

- 'COALESCE' - returns first argument which is not null, if all are null then returns null
```sql
SELECT COALESCE(value1, value2,...,);
```

- 'CAST' - casting data types
```sql
SELECT CAST('5' AS INTEGER);
```

- 'NULLIF' - takes 2 args, if both are equal returns NULL if not then returns the first arg.
``` sql
NULLIF(arg1, arg2);
```
- 'VIEW' - creating kind of a block holder of a query
```sql
CREATE VIEW AS view_name
SELECT column FROM table
...
...;

**call it with**

SELECT * FROM view_name;

DROP VIEW view_name;
```