---
title: Self-Study-SQL-Notebook
date: 2019-03-27 11:07:56
tags:
categories:
- Programming
---

SQL is a very simple modular query language. I have a bad memory that sometimes I really need a cheatsheet ~_~

## PostgreSQL Fundamental

### SELECT

1) select all columns
SELECT * FROM table;

2) select some columns
SELECT column1,column2,... FROM table;

3) select distinct statement in a table
SELECT DISTINCT column1,column2,...FROM table;

4) select where statement
SELECT column1,column2,... FROM table 
WHERE column1 <>= m AND column2 >= n ;

### COUNT

1) count does not consider NULL
SELECT COUNT(DISTINCT column) FROM table;

### LIMIT

1) specify how many rows to select
SELECT * FROM table
LIMIT 5;

### ORDER BY

SELECT column_1,column2 FROM table_name
ORDER BY column_1 ASC,
column_2 DESC;

### BETWEEN

SELECT column_1,column2 FROM table_name
WHERE column_1 BETWEEN m AND n;

### IN

SELECT column_1, column2 FROM table_name
WHERE column_1 IN (1,2)
ORDER BY column_2 DESC;

### LIKE

1)
SELECT column_1,column_2 FROM table_name
WHERE column_1 LIKE 'xxx%';

2)sensitive to case characters
SELECT column_1,column_2 FROM table_name
WHERE column_1 ILIKE '%xxx';

3)
SELECT column_1,column_2 FROM table_name
WHERE column_1 NOT LIKE '%xxx%';

## GROUP BY Statements

### MIN, MAX, AVG, SUM

1) 
SELECT ROUND(AVG(column_1),m) FROM table_name;

2)
SELECT ROUND(MIN(column_1),m) FROM table_name;

3)
SELECT ROUND(MAX(column_1),m) FROM table_name;

4)
SELECT ROUND(SUM(column_1),m) FROM table_name;

### GROUP BY

1) 
SELECT column_1, SUM(column_2) FROM table_name
GROUP BY column_1
ORDER BY SUM(column_2) DESC;

2)
SELECT column_1, COUNT(*) FROM table_name
GROUP BY column_1;

3)
SELECT column_1, COUNT(column_1), SUM(column_1)
FROM table_name
GROUP BY column_1;

### HAVING

1) The condition applies to the group rows created by the GROUP BY, but WHERE applies before GROUP BY
SELECT column_1, aggregate_function(column_2)
FROM table_name
WHERE column_1 IN ("","","")
GROUP BY column_1
HAVING condition;

## JOINS

### AS 

1) 
SELECT column_1, SUM(column_2) AS name
FROM table_name
GROUP BY column_1; 

### Inner Join

1) 
SELECET column_1, column_2, column_3, column_4
FROM table1_name
INNER JOIN table2_name ON table1_name.column_1 = table2_name.column_1;

2) 
SELECT column_1, column_2 AS name_1
FROM table1_name
JOIN table2_name AS name_2 ON name_2.column_1=table1_name.column_1;

### FULL OUTER JOIN

1) The set of all records in table 1 and table 2, the missingness will contain NULL
SELECT * FROM table1_name
FULL OUTER JOIN table2_name
ON table1_name.column_1 = table2_name.column_2

### LEFT OUTER JOIN

1) A complete set of records from table 1 and the matching records in table 2, the missingness of right table will be NULL
SELECT * FROM table1_name
LEFT OUTER JOIN table2_name
ON table1_name.column_1 = table2_name.column_2

2) the records only in table 1 but not in table 2
SELECT * FROM table1_name
LEFT OUTER JOIN table2_name
ON table1_name.column_1 = table2_name.column_2
WHERE table1_name.id IS null

### FULL OUTER JOIN

1) the records unique to table 1 and table 2
SELECT * FROM table1_name
FULL OUTER JOIN table2_name
ON table1_name.column_1 = table2_name.column_2
WHERE table1.id IS null OR table2.id IS null

### UNION

1) all records in table 1 and table 2 even some are same
SELECT column_1,column_2
FROM table1_name
UNION ALL
SELECT column_1,column_2
FROM table2_name;

2) removes all duplicate rows
SELECT column_1,column_2
FROM table1_name
UNION
SELECT column_1,column_2
FROM table2_name;

## Timestamps and Extract

1) Extract day
SELECT extract(day from column_1) FROM table_name;
2) Extract month
SELECT extract(month from column_1) FROM table_name;

## Mathematical Functions

1) addition
SELECT column_1+column_2 AS newcolumn
FROM table_name;

2) multiply
SELECT column_1*column_2 AS newcolumn
FROM table_name;

3) divition
SELECT column_1/column_2 AS newcolumn
FROM table_name;

## String Functions

1) 
SELECT column_1 || ' ' || column_2 FROM table_name;

2)
SELECT lower(column_1) FROM table_name;

## Subquery

1)
SELECT column_1, column_2, column_3 FROM table_name
WHERE 
column_3 > (SELECT AVG(column_3) FROM table_name);

2)
SELECT table1_name.column_1
FROM table1_name
INNER JOIN table2_name ON table1_name.column_1 = table2_name.column_1
WHERE
column_2 BETWEEN '' AND '';

## Self Join

1) Subquery
SELECT column_1 FROM table1_name
WHERE column_2 IN
(SELECT column_2 FROM table2_name)
WHERE column_1 = " ") 

2) Self join
SELECT table1_name.column_1
FROM table1_name AS newname1, table1_name AS newname2
WHERE
newname1.column_2 = newname2.column_2
AND newname2.column_1=" ";

## Creating Databases and Tables

1) Data types
character:char(n)
integers: int(n)
real: float(n)
numeric: numeric(n)

2) Create table
CREATE TABLE table_name (
    column_name data_type column_constraint,
    table_constraint)
INHERITS existing_table_name;

3) INSERT
INSERT INTO table(column1,column2)
VALUES (value1,value2),
VALUES (value1,value2);

#Insert from another table
INSERT INTO table
SELECT column1,column2,...
FROM another_table
WHERE condition;

4) UPDATE
UPDATE table
SET column1 = value1,
    column2 = value2,...
WHERE condition
RETURNING column1, column2,...;

5) DELETE
DELETE FROM table
WHERE condition

6) Alter Table
ALTER TABLE table_name DROP COLUMN column_1;
ALTER TABLE table_name ADD COLUMN column_1 boolean;
ALTER TABLE table_name RENAME COLUMN column1 TO new_column1_name;

7) Drop Table
DROP TABLE IF EXISTS table_name RESTRICT;

8) Check Constraint
CREATE TABLE table_name(
	column_1 serial PRIMARY KEY,
	column_2 VARCHAR(50),
	column_3 DATE CHECK(column_3 > '1900-01-01'),
	column_4 DATE CHECK(column_4 > column_3),
	column_5 integer CHECK(column_5>0)
);

